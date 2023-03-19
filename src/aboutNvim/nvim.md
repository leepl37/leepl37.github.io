# about Nvim and Linux

### mac 

"command + ` " - 동일한 어플리케이션 스위치 , very useful. 


### Nvim 

nvim 으로 rust 코드를 짜기 시작한 후로 점점 더 linux 환경과 nvim에 익숙해진다. 여러모로 도움도 되고 무엇보다 잼희있다. 


다 기억할 순 없으니 생각에 남는 부분은 정리하다. 


#### 변경(replace)

```vi
:1, 5s/^/hello 
```

* 첫째줄과 다섯째줄 맨 앞의 hello 로 추가.
* ^ 는 맨 앞이라는 정규식 의미이고 마찬가지로 $ 맨 뒤.
* first character -> *. : 1st char, *.. til 2nd char .. 

```vi
:%s/text/willchange/g
```

* 전체 내용에서 text라는 내용을 찾고 willchange로 변경한다.
* 뒤에 g 를 붙이면 global, 전체가 한번에 바뀜. 

#### 찾기(find)

```vi
:vimgrep /pattern/g % -> current page, all lines search.   
:vimgrep /pattern/g **/* -> directory search
:vimgrep/text/g *.rs 
:vimgrep pattern/g ../**/* -> upper directory
```

* vi의 자체 기능이며 썩 만족스럽지 않다. 
* 찾을 문구 넣고 /g 전체 *.rs 해당 파일 중에 검색. 

* most used command in vimgrep 
	* vimgrep pattern * - This command searches for pattern in all the files in the current directory and its subdirectories. The search results are displayed in a quickfix window.

	* vimgrep pattern file1 file2 file3 - This command searches for pattern in the specified files (file1, file2, and file3 in this example). The search results are displayed in a quickfix window.
	* copen - This command opens the quickfix window, which displays the search results.
		* shortcut for cw 

	* cnext - This command moves the cursor to the next search result in the quickfix window.
	* cprev - This command moves the cursor to the previous search result in the quickfix window.
	* cfirst - This command moves the cursor to the first search result in the quickfix window.
	* clast - This command moves the cursor to the last search result in the quickfix window.
	* cclose - This command closes the quickfix window.
	* vimgrepadd pattern * - This command adds to the existing search results in the quickfix window. It searches for pattern in all the files in the current directory and its subdirectories, and adds the results to the quickfix list.
	* vimgrepadd pattern file1 file2 file3 - This command adds to the existing search results in the quickfix window. It searches for pattern in the specified files (file1, file2, and file3 in this example), and adds the results to the quickfix list.



#### Undo Redo

* Undo, 일반모드에서 u 
* Redo, 일반모드에서 ctrl + r 

#### Visiul block 

* ctrl + q
* visual block 이란 다른 ide tool 에서 라인을 세로 블록으로 하고 수정이 가능하도록 하는 기능이다. 



