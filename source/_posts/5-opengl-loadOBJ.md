title: (C++\openGL)讀取.obj模型檔
date: 2014-07-23 19:15:11
tags: openGL
category:
- 3D圖學、openGL與遊戲製作
---

本篇文章主要會說明如何用C++自己寫一個簡單的obj loader，並用openGL顯示

##讀取obj檔
###建立一個obj檔

首先第一步就來建立一個obj檔吧！因為obj檔的格式會因為模型裡包含的資訊而有所不同，
為了方便理解，就先大概提一下obj檔的建立流程吧~在這裡我使用的3D軟體是blender，一
個很不錯且免費的3D建模軟體。[(官網點我)](http://www.blender.org/)

<!--more-->

打開blender應該會看到類似這樣的畫面：
![](/image/post5-blender1.png)

因為我們的目標只是讀obj檔，不需要多好看的模組，不過假如只有預設的方塊又好像有點弱。
所以就用blender內建的猴子頭模組吧！
先用右鍵點選中間的方塊，按delete把它刪除，接著看視窗旁邊有一欄工具列，按create的標籤
，mesh的下方就有個猴子頭的選項，點一下，猴子頭就跑出來了！

![](/image/post5-blender2.png)
(注意過程中左鍵不要亂點，不然準星可能會亂掉優)

接著就直接輸出猴子頭的obj檔吧！左上方file有個export的選項，然後點obj。

![](/image/post5-blender3.png)

選擇你想要的路徑，不過最後輸出之前有些選項要注意一下：
![](/image/post5-blender4.png)

紅框圈起來的地方要記得勾選，一個是把所有的面分成三角形，不這樣的話會有不同的形狀混雜，
讀起來很麻煩；另一個是法線資訊，假如要打光就會用到，為了統一格式也把它勾起來吧~

最後按下右上角的export obj就完成了！

###obj檔的格式
接著到你輸出的位置用文字編輯器打開你的obj檔，你應該會看到類似這樣的內容：
```bash
# Blender v2.70 (sub 0) OBJ File: ''
# www.blender.org
mtllib untitled.mtl
o Suzanne
v 0.437500 0.164062 0.765625
v -0.437500 0.164062 0.765625
v 0.500000 0.093750 0.687500
v -0.500000 0.093750 0.687500
...
```
還有這樣
```bash
vn 0.671345 -0.197092 0.714459
vn -0.671345 -0.197092 0.714459
vn 0.832580 -0.301659 0.464556
vn -0.832580 -0.301659 0.464556
...
```
最後是這樣
```bash
f 47//1 1//1 3//1
f 4//2 2//2 48//2
...
f 489//917 491//917 471//917
f 468//918 466//918 490//918
f 469//919 491//919 467//919
...
```
這些是什麼意思呢？這裡大略說明一下：
v開頭的是指vertex，後面接的數字是頂點的座標位置
vn則是vertex normal，是一個向量，表示頂點的法向量
f是face，代表面的資訊，它的格式及本上是：vertex/texture coordinate/normal
因為我們現在沒有貼圖，就變成vertex//normal這樣的格式，而數字的意思則是編號，
像第一個出現的v就是一號頂點，第一個vn就是一號法向量，而像這樣
f 47//1 1//1 3//1
就表示這個面由三個點組成，分別是：
第一個點：47號位置，1號法向量
第二個點：1號位置，1號法向量
第三個點：3號位置，1號法向量
畢竟是同一面，法向量一樣也是理所當然的，因為我們輸出時已經都分成三角形了，所以基本上所有資料都應該是三個一組

這樣應該對obj檔有了基本的了解，接下來就開始就用c++來寫我們的loader吧！

##用c++寫obj loader
(這裡大部分是參考[這裡](http://www.opengl-tutorial.org/beginners-tutorials/tutorial-7-model-loading/)的教學，有興趣的人也可以去看看優~~)

這裡我們要做的事非常簡單，就是把所有頂點跟法向量的資料都存到記憶體裡面，才能再傳給openGL繪圖，
這裡我會使用兩個vector來存它，那麼就開始吧！
個人習慣會先建立一個class，把讀檔的函式取名叫LoadObjFromFileVertexAndNormal，有點長......取這個
名字的原因是考慮到之後你可能會需要其他版本來讀不同的格式
```bash
class ObjLoader {
public:
	ObjLoader();
	bool LoadObjFromFileVertexAndNormal(const char*,std::vector<float>*,std::vector<float>*);
};

```
接著進到我們的函式內容：
```bash
bool ObjLoader::LoadObjFromFileVertexAndNormal(const char* path,std::vector<float>* vertices
		,std::vector<float>* normals) {

	char lineHeader[1000];
	std::vector<float> temp_vertices;
	std::vector<float> temp_normals;
	std::vector<unsigned int> indices;
	float vertex[3], normal[3];
	unsigned int index[3][3],f=0;

	FILE* objfile = fopen(path, "r");
	if (objfile == NULL) {
		printf("Impossible to open the file !\n");
		return false;
	}
```
如你所見，我們的函式會吃三個參數，一個是檔案路徑，另外兩個是vector的指標，就是等下我們存資料的地方。
接下來是變數的宣告，它們的功能之後遇到再說明吧。
接下來是用fopen打開檔案，這部分應該沒什麼問題吧，如果找不到檔案，就印出錯誤訊息並結束。
再來是讀取內容的主要部分：
```bash
while (fscanf(objfile, "%s", lineHeader) != EOF) {
		if (strcmp(lineHeader, "v") == 0) {

			fscanf(objfile, "%f %f %f\n", &vertex[0], &vertex[1], &vertex[2]);
			temp_vertices.push_back(vertex[0]);
			temp_vertices.push_back(vertex[1]);
			temp_vertices.push_back(vertex[2]);
		}
		else if (strcmp(lineHeader, "vn") == 0) {

			fscanf(objfile, "%f %f %f\n", &normal[0], &normal[1], &normal[2]);
			temp_normals.push_back(normal[0]);
			temp_normals.push_back(normal[1]);
			temp_normals.push_back(normal[2]);
		}
		else if (strcmp(lineHeader, "f") == 0) {

			int matches = fscanf(objfile, "%u//%u %u//%u %u//%u",
					&index[0][0], &index[2][0], &index[0][1],&index[2][1], &index[0][2], &index[2][2]);
			printf("matches=%d,f=%u\n",matches,f++);

			if (matches != 6) {
				printf("File can't be read by our simple parser ( Try exporting with other options\n");
				return false;
			}
			indices.push_back(index[0][0]);
			indices.push_back(index[2][0]);
			indices.push_back(index[0][1]);
			indices.push_back(index[2][1]);
			indices.push_back(index[0][2]);
			indices.push_back(index[2][2]);
		}
	}
```
這裡就是一直fscanf開頭的字串，對應不同字(v,vn或f)做對應的處理，注意這裡我們是存到暫時的vector裡，因為
這裡只是每個頂點和法向量的資料，還沒有照每個面的順序排過。
喔，還有一點就是原本我看的教學是用glm這個函式庫的資料結構vec3和vec2來存，但是因為我希望盡量減少外部的
相依性，所以在這裡我都直接用float，看起來可能比較亂一點就是了

最後就是依照面的順序存到我們最後真正使用的vector裡：
```bash
for (unsigned int i = 0; i < indices.size(); i+=2) {
		vertices->push_back(temp_vertices[(indices[i] - 1) * 3]);
		vertices->push_back(temp_vertices[(indices[i] - 1) * 3 + 1]);
		vertices->push_back(temp_vertices[(indices[i] - 1) * 3 + 2]);
		normals->push_back(temp_normals[(indices[i + 1] - 1) * 3]);
		normals->push_back(temp_normals[(indices[i + 1] - 1) * 3 + 1]);
		normals->push_back(temp_normals[(indices[i + 1] - 1) * 3 + 2]);
	}
```
這部分應該是最容易搞錯的部分，首先，indices這個vector裡存的是vertex和normal的編號，我們要再照編號
找出對應的資料。
(例如：第一個面在obj檔裡是這樣
	f 47//1 1//1 3//1
indices裡的順序就是47,1,1,1,3,1
代表第一個點是在47號的座標，因為vector是從0開始，所以它是在46的地方，而因為每個座標是由3個float組成，
所以46號的位置是由temp_vertex裡的46*3,46*3+1,46*3+2三個位置的float代表的，而normal也是這樣，仔細推敲
應該看得懂吧......)

這樣就完成obj檔的轉換了~~接下來就是用openGL繪圖囉~~

###用openGL顯示3D model

(注意:這裡使用的是openGL 3.0之後的語法，非舊版的)
先貼個code:

```c++
	std::vector<float> vertices, normals;
	obj_loader.LoadObjFromFile("ship00.obj", &vertices,&normals);
	  
	GLuint vbo_points = 0;
	glGenBuffers(1, &vbo_points);
	glBindBuffer(GL_ARRAY_BUFFER, vbo_points);
	glBufferData(GL_ARRAY_BUFFER, vertices.size() * sizeof(float), &vertices[0],GL_STATIC_DRAW);
	  
	glEnableVertexAttribArray(0);
	glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 0, NULL);
      
	GLuint vbo_normals = 0;
	glGenBuffers(1, &vbo_normals);
	glBindBuffer(GL_ARRAY_BUFFER, vbo_normals);
	glBufferData(GL_ARRAY_BUFFER, normals.size()* sizeof(float), &normals[0], GL_STATIC_DRAW);
          
	glEnableVertexAttribArray(1);
	glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, 0, NULL);
           
	GLuint matrix_location = glGetUniformLocation(shader_programme, "MVP");
	GLuint model_matrix_location = glGetUniformLocation(shader_programme, "M");
	GLuint view_matrix_location = glGetUniformLocation(shader_programme, "V");
                       
	while (!glfwWindowShouldClose(window)) {

		glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
		glUseProgram(shader_programme);

		glm::mat4 MVP = Projection * View * Model;

		glUniformMatrix4fv(matrix_location, 1, GL_FALSE, &MVP[0][0]);
		glUniformMatrix4fv(model_matrix_location, 1, GL_FALSE, &Model[0][0]);
		glUniformMatrix4fv(view_matrix_location, 1, GL_FALSE, &View[0][0]);
               
		glDrawArrays(GL_TRIANGLES, 0, vertices.size());
                       
		glfwPollEvents();
		glfwSwapBuffers(window);
	}

```
這裡是把之前的code包裝在ObjLoader這個class裡面了，其他就是很簡單的用glBufferData傳vector內的資料給
GPU，然後畫出來就是了。

至於對這段code完全無法理解的人，之後應該會有一篇文章介紹openGL 3.0之後的基礎用法，現在就先了解如何把
一個簡單的obj檔轉換成c++的vector資料結構吧！

英文不錯的人也可以去我在第一段提到的網站看看，前面也有一些基礎的教學喔 QWQ

