~~~css
 input[type=checkbox] {
    width: 15px;
    height: 15px;
    margin-right: 10px;
    position: relative;
  }
  input[type=checkbox]::after {
    position: absolute;
    top: 0;
    color: #000;
    width: 15px;
    height: 15px;
    display: inline-block;
    visibility: visible;
    padding-left: 0;
    text-align: center;
    content: ' ';
    // border-radius: .03rem;
  }
  input[type=checkbox]:checked::after {
    content: "✓";
    color: #fff;
    font-size: 10px;
    line-height: 15px;
    background-color: red;
    
  }
~~~

![[../img/Pasted image 20231117115032.png]]

![[../img/Pasted image 20231117115042.png]]

~~~css
.custom-checkbox {
  /* 隐藏默认的复选框 */
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;

  /* 定义复选框的大小和边框 */
  width: 16px;
  height: 16px;
  border: 2px solid #000;
  border-radius: 4px;
  outline: none;

  /* 定义未选中状态下的背景颜色 */
  background-color: #fff;
}

/* 定义选中状态下的背景颜色 */
.custom-checkbox:checked {
  background-color: #000;
}

/* 定义鼠标悬停时的背景颜色 */
.custom-checkbox:hover {
  background-color: #ccc;
}

~~~