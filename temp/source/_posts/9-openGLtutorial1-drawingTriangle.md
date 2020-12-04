title: openGL教學<二> 繪製三角形
date: 2014-10-15 07:49:40
tags: 
- openGL 
- c++
category:
- 3D圖學、openGL與遊戲製作
---

###使用openGL的第一步###

這次的教學要談的是如何用openGL與C++的程式畫出一個最簡單的三角形！
不過，在正式開始coding前，我們還有一些事要處理......

<!--more-->

###設定環境###

沒錯，設定環境，要讓我們的程式使用openGL，這是我們一開始必須做的事。值得慶幸的是，我們需要做的事並不會很多。 
以下簡單做個介紹：

1.GLFW
-到[GLFW的官網](http://www.glfw.org/download.html)下載最新版本的檔案，如果你是在windows的系統下，可以直接下載
 Windows pre-compiled binaries的檔案
-解壓縮之後，裡面應該會出現一些資料夾。
-將include裡的GLFW資料夾丟到你的系統的inclued路徑裡(像我就是C:\MinGW\include)
-視你的環境從名字有lib的資料夾裡選一個，把裡面的檔案丟到系統的lib路徑裡(像我就是C:\MinGW\lib)
-OK！

2.GLEW
-步驟跟GLFW差不多，直接下載完檔案，把它們丟到合適的位置就好了！

3.連接library

該下載完的東西都下載完後，接著就可以開啟你的IDE準備開始寫code了！
但在那之前，你需要連結你的函式庫到你的專案，我在eclipse CDT的環境下看起來像這樣

![link library](/image/link_library.jpg)

如果你沒有使用IDE，在編譯時記得加上 -lglfw3 -lopengl32 -lglew32 -lgdi32 


###開始寫code吧！###

很好，現在環境設定完畢，終於開始寫code了！今天我們的目標就是產生一個視窗，並在視窗裡畫一個三角形！
先來看我們的第一段code吧！

###初始化###

```c++
#include <GL/glew.h> // include GLEW and new version of GL on Windows
#include <GLFW/glfw3.h> // GLFW helper library
#include <glm/glm.hpp>
#include <glm/gtc/matrix_transform.hpp>
#include <cstdio>
#include <cmath>
#include "MyShader.h"
    
int main() {

	if (!glfwInit()) {  //檢查GLFW是否正常被初始化
		fprintf(stderr, "ERROR: could not start GLFW3\n");
		return 1;
	}
  
	GLFWwindow* window = glfwCreateWindow(640, 480, "Helloooo Triangle", NULL,NULL);
	//建立一個GLFW視窗
  
	if (!window) {  //檢查視窗是否正確建立
		fprintf(stderr, "ERROR: could not open window with GLFW3\n");
		glfwTerminate();
		return 1;
	}
	glfwMakeContextCurrent(window);  //將我們的視窗視為openGL當前的context
  
	glewExperimental = GL_TRUE;
	glewInit();		//初始化GLEW
  
	const GLubyte* renderer = glGetString(GL_RENDERER); 
	const GLubyte* version = glGetString(GL_VERSION); 
	printf("Renderer: %s\n", renderer);
	printf("OpenGL version supported %s\n", version);	//檢查繪圖裝置與openGL支援的版本

```

這段程式碼主要做的就是初始化的工作~~
一開始把我們需要的標頭檔include進來，這應該沒什麼問題！

接著就是檢查GLFW和GLEW，開啟視窗並確認當前openGL的版本，這部分的寫法應該就是固定這樣，也沒什麼
特別值得說明的。大概就是看一下glfwCreateWindow的參數，控制一開始的視窗寬高和名稱。另外一個函式
glfwMakeContextCurrent的功能可能也不是很一目瞭然，簡單來說openGL要開始進行繪圖的話，要先有一個
context作為目標，就像畫布一樣，之後的繪圖就會在這個context上進行！

接著繼續看下去~~

###3維空間MVP矩陣與頂點資訊###

```c++
  
	glEnable(GL_DEPTH_TEST);	//深度測試，讓近的東西蓋掉遠的
    
	glm::mat4 Projection = glm::perspective(45.0f, 4.0f / 3.0f, 0.1f, 100.0f);
  	//投影矩陣，後面4個參數分別代表角度、長寬比例、zNear、zFar
    
   
	glm::mat4 View = glm::lookAt(
	    glm::vec3(2,2,4),	//攝影機位置在2,2,4
	    glm::vec3(0,0,0),	//看向0,0,0的位置
	    glm::vec3(0,1,0)	//代表向上的向量
	);
	//
  
	glm::mat4 Model = glm::mat4(1.0f);	//模型矩陣，這裡沒有動到模型，因此是單位矩陣
	glm::mat4 MVP = Projection * View * Model;	//MVP矩陣，注意矩陣乘法是反過來的
   
   
	GLfloat points[] = {	//三角形的頂點位置
		     1.0f, 1.0f, 1.0f, 
		    -1.0f,-1.0f, 1.0f,
		    -1.0f, 1.0f, 1.0f 
		};
	GLfloat colors[] = {	//頂點顏色
		    0.583f,  0.771f,  0.014f,
		    0.609f,  0.115f,  0.436f,
		    0.327f,  0.483f,  0.844f
		};
```
這一段我們所做的事就是宣告等下要用的資料。

一開始的glEnable(GL_DEPTH_TEST);是為了讓openGL知道遠近，讓近的蓋掉遠的，雖然現在我們只畫一個三角形
可能看不出來，但就先留著他吧！

接著我們看到了glm這個函式庫的登場，在glm的輔助下，我們產生了projection、view和model三個矩陣，這三個
矩陣在3D繪圖中可是扮演著不可或缺的角色！在這篇文章中先不細談他們的用途，之後應該會介紹。
(如果很有興趣的話可以先參考[這裡]())

最後是兩個陣列，都是三個資料一組，代表著三角形頂點的位置與顏色。

###VBO與VAO###

繼續看下去~
  
```c++
	unsigned int points_vbo = 0;	//宣告頂點位置vbo
	glGenBuffers(1, &points_vbo);
	glBindBuffer(GL_ARRAY_BUFFER, points_vbo);	//綁定vbo
	glBufferData(GL_ARRAY_BUFFER, 108*sizeof(float), points, GL_STATIC_DRAW);	
	//將陣列points的值傳給vbo
   
	unsigned int colors_vbo = 0;	//宣告頂點顏色vbo
	glGenBuffers(1, &colors_vbo);
	glBindBuffer(GL_ARRAY_BUFFER, colors_vbo);	//綁定vbo
	glBufferData(GL_ARRAY_BUFFER, 108*sizeof(float), colors, GL_STATIC_DRAW);
	//將陣列colors的值傳給vbo
   
	unsigned int vao = 0;		//宣告vao
	glGenVertexArrays(1, &vao);
	glBindVertexArray(vao);		//綁定vao
   
	glBindBuffer(GL_ARRAY_BUFFER, points_vbo);
	glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 0, NULL);
	glEnableVertexAttribArray(0);
 
	glBindBuffer(GL_ARRAY_BUFFER, colors_vbo);
	glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, 0, NULL);
	glEnableVertexAttribArray(1);
```
這段可以說是比較複雜的部份，這裡做個大略的說明：

前面兩段分別為我們的兩個陣列(分別代表頂點位置與顏色)創建了一個vbo。
所謂的vbo，全名vertex buffer object，可以把它想成GPU裡面的一段記憶體區塊，用來存放跟頂點有關的資訊。
也許有人還不知道，在電腦繪圖的過程中，從顯卡的GPU裡存取資料是遠比從電腦的CPU存取還要快上許多，而openGL
就讓我們能夠以這種方式直接對GPU的記憶體內容來做操作，但相對的在語法上比較複雜一些，也比較難debug。

一開始我們宣告一個unsigned int來當作vbo的ID，接著用glGenBuffers表示新增一個vbo，其中第一個參數是你要
新增幾個vbo，注意如果你要宣告多個vbo的話，一開始你得宣告unsigned int的陣列才行，至於第二個參數就是你
前面宣告ID的位址。

接著來看glBindBuffer(GL_ARRAY_BUFFER, colors_vbo)，這個函式的功用就是綁定你的vbo到指定的綁定點上。在這裡
我們的綁定點是GL_ARRAY_BUFFER，第二個參數就是綁定的vbo的ID。openGL提供了許多綁定點，但在這裡不詳細說明，
先記住最重要的一點：不論是vbo或之後會提的vao，你都必須先綁定才能對它做操作！

接著就是把CPU的資料傳到GPU裡了！以C的概念來看，一開始的glGenBuffers可以說我們有了一個指向GPU的指標，現在
我們就要在這個指標的位置配置一塊記憶體，並將CPU的資料傳輸過去，而這正是glBufferData所做的事。第一個參數是
綁定點，目前也是GL_ARRAY_BUFFER，第二個是你要配置的記憶體大小，再來是CPU資料的指標，這些問題應該都不大！
至於最後一個參數稍微有點意思，它影響的是你的這塊記憶體會被配置在GPU的哪個位置，我們來看看三個選擇：

GL_STATIC_DRAW，也就是我們現在用的，表示這塊記憶體幾乎不會需要更新。
GL_STREAM_DRAW，表示在每次渲染時，這塊記憶體都可能需要更新。
GL_DYNAMIC_DRAW，可能每次畫面都會更新，甚至更新多次，可能某些特殊動畫相關才會用到。

一般的模型(包括我們要畫的三角形)應該都不會頻繁變動資料，用GL_STATIC_DRAW就好了！

最後兩段則提到vao的建立。vao，全名vertex array object，通常我們會把彼此關聯的vbo綁定在一個vao上，在繪圖時
就可以一次進行。而vao的宣告跟綁定和vbo相差不多，就不再重新說明了。

但是glVertexAttribPointer這個重要的函式還是要說明一下，它決定了程式如何解讀記憶體裡面的資料。其中第一個
參數是索引值，跟之後shader(著色器)有關，目前先注意不要重複就好；第二個參數是說幾個資料是一組的，向我們的
points陣列是三個一組，分別代表x,y,z座標，顏色也三個一組(RGB)，因此第二個參數都是3；第三個參數是資料型態，
我們都是float應該沒問題；第四個參數代表是否要normalize，一般來說設GL_FALSE就好；第五個參數是每組資料間的
偏移量，目前是0；最後則是起始位置的指標，我們的話就是陣列開頭的指標！

設定好之後就要glEnableVertexAttribArray，參數跟glVertexAttribPointer的第一個參數一樣！

不知道會不會很難吸收 QWQ
筆者一開始也被一堆函示搞的很混亂，可以先照著做，之後再慢慢吸收~~

###著色器(SHADER)###

再來看下一段code

  
```c++
	GLuint VertexShaderID = glCreateShader(GL_VERTEX_SHADER);		//宣告VertexShader的ID
	GLuint FragmentShaderID = glCreateShader(GL_FRAGMENT_SHADER);	//宣告FragmentShader的ID
   
	const char *vertex_shader =						//VertexShader的原始碼，為了方便這裡以字串表示
			"#version 400\n\
			layout(location = 0) in vec3 vertex_position;\n\
			layout(location = 1) in vec3 vertex_color;\n\
			uniform mat4 MVP;\n\
			out vec3 color;\n\
			void main () {\n\
					color = vertex_color;\n\
					gl_Position = MVP*vec4 (vertex_position, 1.0);\n\
			}";
	glShaderSource(VertexShaderID, 1, &vertex_shader , NULL);		//指明VertexShader的原始碼來源
	glCompileShader(VertexShaderID);								//編譯VertexShader
  
	const char *fragment_shader =	  					//FragmentShader的原始碼，為了方便這裡以字串表示
				"#version 400\n\
				in vec3 color;\n\
				out vec4 frag_color;\n\
				void main () {\n\
						frag_color = vec4 (color, 1.0);\n\
				}";
	glShaderSource(FragmentShaderID, 1, &fragment_shader , NULL);	//指明FragmentShader的原始碼來源
	glCompileShader(FragmentShaderID);								//編譯FragmentShader
 
  
	unsigned int shader_programme = glCreateProgram();		//建立shader program
	glAttachShader(shader_programme, vertex_shader);		//把shader attach到shader program上
	glAttachShader(shader_programme, fragment_shader);
	glLinkProgram(shader_programme);						//綁定shader program
```
這裡對使用openGL而言也是非常重要的一段！

在這裡，我們建立了在openGL中扮演非常重要角色的shader(著色器)。openGL中有許多種shader，其中最基本的就是我們現在建立的
vertex shader(頂點著色器)跟fragment shader(片段著色器)了！

從程式碼中我們可以看到一段很奇怪的字串，裡面還有像void main()這樣的關鍵字，好像也是一段程式碼一樣。沒錯！其實openGL的
shader是用一種跟c十分相似的語言GLSL來編寫的，而我們的vertex_shader和fragment_shader這兩個字串，正是我們的兩個shader的
原始碼！在這裡為了方便，我們是直接把原始碼塞到一個字串裡，其實這樣寫起來明顯的不太方便，把shader的原始碼寫在另外的檔案
裡再讀進來其實會是個比較好的做法，不過這裡先將就一下吧！

我們一樣宣告shader的ID，用glShaderSource把原始碼傳給它，並用glCompileShader編譯。
之後再建立一個shader program，把我們寫好的shader attach上去，這樣在繪圖時就可以用我們的shader program來繪圖了！

至於GLSL的原始碼所代表的意義，我想......還是之後跟openGL的渲染管線一起做說明好了！不然一知半解應該也不太好。

###GLFW主迴圈與繪圖###

來看最後一段code吧！
  
```c++
	GLint matrix_location = glGetUniformLocation(shader_programme, "MVP");
  
	while (!glfwWindowShouldClose(window)) {
  
		glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);		//清理buffer
		glUseProgram(shader_programme);							//使用shader program
		glUniformMatrix4fv(matrix_location, 1, GL_FALSE, &MVP[0][0]);	//傳送uniform值給shader
		glBindVertexArray(vao);											//綁定vao
		glDrawArrays(GL_TRIANGLES, 0, 3);								//繪圖！
		glfwPollEvents();						
		glfwSwapBuffers(window);										//刷新畫面
	}
  
	glfwTerminate();
	glDeleteShader(vertex_shader);
	glDeleteShader(fragment_shader);
  
	return 0;
}

```

這是最後一段程式碼了！加油~

一開始我們宣告了一個matrix_location的變數。還記得剛剛的shader吧，在openGL中我們大部分的運算可能都會交給shader來處理，
畢竟像之前提到的，GPU是比CPU還快的。但是sahder要怎麼知道在外面我們c++的一些變數呢(就像我們一開始的MVP矩陣)？這就是這個
glGetUniformLocation跟後面的glUniformMatrix4fv的用意了。

glGetUniformLocation的第一個參數告訴你是哪個program，第二個參數是你要給他的變數名稱。注意這裡的變數名稱跟shader裡宣告的
變數名稱必須相等！後面的glUniformMatrix4fv就是把實際的值傳過去，關於細節就之後跟shader一起講吧！

哇哇哇，最後的最後，讓我們來看這個while迴圈吧！這裡可以看到while (!glfwWindowShouldClose(window))，就是當視窗還是開著的
時候，迴圈就會一直執行。我們會先用glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)這行把視窗暫存器中的東西都clear掉，
然後使用我們剛剛建立的shader program，傳遞shader需要的uniform值，綁定vao，繪圖並刷新畫面。

最後，當迴圈跳出結束時，就把東西清一清，return！

講解完畢，趕快來執行看看吧！

![helloooooooo triangle](/image/hello_triangle.jpg)

執行結果如圖所示！一個漂亮的三角形就這樣出現了~~

可能這篇文講了太多東西，也不是很清楚，但希望大家有個對openGL程式是如何撰寫的有個基本的概念！
之後還會有文章更詳細的說明每部分，包括3D空間的矩陣運算、openGL的渲染過程和shader的原理等等~~

這次就先到這裡吧！