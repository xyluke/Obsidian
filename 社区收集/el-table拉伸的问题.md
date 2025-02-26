![[../img/Pasted image 20250226174739.png]]

~~~js
::v-deep .el-table{
    width: 100%;
    .el-table__header-wrapper table,.el-table__body-wrapper table{
      width: 100% !important;
    }
    .el-table__body, .el-table__footer, .el-table__header{
      table-layout: auto;
    }
  }
~~~