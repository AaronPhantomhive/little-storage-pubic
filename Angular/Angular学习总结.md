# Angular学习总结

Angular和react，vue相似，都是轻量级框架。

组件库网站：https://material.angular.cn/

ag-grid网格框架：https://www.ag-grid.com/angular-data-grid/

第三方依赖包：npmjs.com

## 环境

### 创建工程指令

Angular CLI 工具包 + Node.js（类似java的JDK）

安装angular CLI: npm install -g @angular/cli (加版本的话后面+@x.x←版本号)

查看版本：ng --version

help: ng help

创建：ng new angular-start

serve: ng serve

prettier: npm install prettier prettier-eslint eslint-config-prettier eslint-plugin-prettier --save-dev

- 把以下加入package.json的scripts，然后run，进行全局格式化

  ```json
  "prettier:write": "prettier --write ."
  ```

- 常用prettier配置

  ```json
  {
    "printWidth": 120,
    "useTabs": false,
    "tabWidth": 2,
    "semi": true,
    "singleQuote": true,
    "quoteProps": "as-needed",
    "trailingComma": "all",
    "bracketSpacing": true,
    "bracketSameLine": false,
    "arrowParens": "always",
    "endOfLine": "crlf",
    "overrides": [
      {
        "files": "**/*.html",
        "options": {
          "trailingComma": "none"
        }
      }
    ]
  }
  ```

- .prettierignore

  ```
  **/node_modules/**
  *.js
  *.d.ts
  *.json
  
  # See http://help.github.com/ignore-files/ for more about ignoring files.
  
  # Compiled output
  /dist
  /tmp
  /out-tsc
  /bazel-out
  
  # Node
  /node_modules
  npm-debug.log
  yarn-error.log
  
  # IDEs and editors
  .idea/
  .project
  .classpath
  .c9/
  *.launch
  .settings/
  *.sublime-workspace
  
  # Visual Studio Code
  .vscode/*
  !.vscode/settings.json
  !.vscode/tasks.json
  !.vscode/launch.json
  !.vscode/extensions.json
  .history/*
  
  # Miscellaneous
  /.angular/cache
  .sass-cache/
  /connect.lock
  /coverage
  /libpeerconnection.log
  testem.log
  /typings
  
  # System files
  .DS_Store
  Thumbs.db
  ```

### 查看工程

**.vscode文件** 配置文件

**node_module** 包文件（依赖包）

**brower** 浏览器支持

**editorconfig** 代码格式支持

**angular.json** 配置文件

**package.json** 装的包（很重要）唯一支持外部交互的文件

**package-lock.json** 版本

**.gitignore** 不被git检测到的文件配置

### 启动服务

package.json 文件里：

start为启动 ng server --port 4300（此为默认端口） ← 任意端口

devDependences 开发版本 npm -i xxx --save-dev

dependences 运行版本

### 将css样式迁移到scss

在angular.json里加入以下

```json
"schematics": {
  "@schematics/angular:component": {
    "style": "scss"
  },
  "@schematics/angular:application": {
    "strict": true
  }
},
```

并将styles的css改成scss（2个）

```json
"styles": [
  "src/styles.scss"
],
```

最后全局替换，rename，重新编译。

## 开发

### src文件夹内介绍

assets 资源文件

enviroment 环境文件

...ico 网站图标

index.html 主页

polyfill.js 兼容

style.css 基础样式文件

test.js 单元测试启动

### src-app 开发主文件夹介绍

html scss ts 重要三文件 

有可能会有service

## 基础组件 & 元素 & 语法

### 元素

module：模块

component：组件

router：路由

- 负责页面跳转

pipe：管道

- 对字符串做一些特定的处理，一般用来改变内容的格式

directive：指令

- 改属性，扩容

service：服务

### 引入模块和服务（module.ts 文件）

模块：`import [……]` 

服务：`providers [……]` 

默认启动组件：`bootstrap [APPComponent]` 

### index.html

组件头：`<app-root></app-root>`

### 路由

为配置路径，设置页面跳转用。

为惰性加载，预加载。

`Router=[{ path: 'xxx', component: xxx, children[{}] }];`

两种方式：隐式路由，路由跳转。隐式路由为不管怎么切换，url都保持不变。

### 隐式路由

```html
[skipLocationChange]="true"
```

### 异步路由

```typescript
const appRouter=[
	{
		path:'',
		loadChildren:'app/home/home.module'
	},{
		path:'home',
		loadChildren:'app/home/home.module'
	},{
		path:'user',
		loadChildren:'app/home/user.module'
	},{
		path:'role',
		loadChildren:'app/home/role.module'
	}
];
```

### 依赖注入

`constructor(private router: Router) {}`

### 路由守卫

判断是否有认证。例如登录认证，权限认证。

## 组件的写法和基础语法

### 生命周期

写法为在class xxxx implements 后面。

eg: `class xxxx implements OnInit, AfterContextInit {}`

angular生命周期为：

OnChanges：当数据绑定输入属性的参数改变的时候调用。
OnInit：在angular第一次显示数据绑定和设置指令/组件的输入属性之后，初始化指令/组件。
DoCheck：自定义的方法，用于检测和处理值的改变。
AfterContentInit：在组件内容初始化之后调用。
AfterContentChecked：组件每次检查内容时调用。
AfterViewInit： 组件相应的视图初始化之后调用。
AfterViewChecked：组件每次检查视图时调用。
OnDestroy：指令销毁前调用。

其中经常调用的，画面加载速度从快至慢为：OnInit，AfterContextInit，AfterViewInit。

**如果页面加载错误，且使用的onInit的话，可以试一下AfterViewInit。**

### 双向绑定

语法：

`[]` 变量 也是单向绑定

`( )` 方法

`[( )]` 双向绑定 

`{{ }}` 差值表达式

双向绑定含指令: `<input [(ngModel)]="inputStr" type... >`  

### 循环，判断

`ngFor`  为forEach

- eg：`<app-xxx ngFor="let aaa of aaas" [aaa]='aaa'>`

`ngIf` 为Boolean表达式

`ngSwitch` 为switch case表达式

### 通过service传值

A.ts

```typescript
constructor(
  private service: Service,
) { }

this.preparationService.xxxx = this.xxxxxxx;
```

service.ts

```typescript
export class Service{
  xxxx: type;
}
```

B.ts

```typescript
constructor(
  private service: Service,
) { }

this.xxxx = this.preparationService.xxx;
```

### @Input 组件之间传值

子组件.ts 里：

`@Input inputParam: any`

- 加一个Input装饰器，为了可以让父组件给其提供值。
- 可以在构造函数里，给`inputParam`赋值。

`@output inputParamChange ...;`

子组件.html 里：

`<input matInput [ngModel]="currentDate" />`

父组件.html 里调用时：

`<xxx-Component [(inputParam)]="inputParamInParentCom"></>`

- sxxx-Component 为子组件的@Component里selector的名字。
- `inputParam` 为调用的子组件的属性（属性绑定）。
- 父组件.ts里，定义`inputParamInParentCom`的值。

跑fun()：`emit();`

### @ViewChild

①在ts里使用html的值

- html里：#xxx变量

- ts里：@ViewChild('xxx') xxx: any;

之后就可以使用xxx在html里面的属性了，例如：this.xxx.disabled

②**父组件把子组件的东西拿过来用。**

- 整个component：通过class名称，@ViewChild(TodoComponent) private todoList: TodoComponent; 然后就可以调用子组件里的方法了。
- 变量：子组件中构造器中接收的Service，一些非private变量。
- DOM：直接获取子组件HTML页面中的DOM，该DOM需要#命名
  - component.html 里：`<mat-sidenav #sidenav>`
  - component.ts 里`@ViewChild('sidenav') sidenav!: MatSidenav;`
- 指令：添加exportAs属性。

注意：@ViewChild和@ViewChildren会在父组件钩子方法 `ngAfterViewInit()` 调用之前赋值。

### BehaviorSubject

BehaviorSubject存储着最后发出给obsever的值。

例子1：

```typescript
var subject = new Rx.BehaviorSubject(0);

subject.subscribe({
	next:(v) => console.log('observerA:' + v);
});

subject.next(1);
subject.next(2); // 发出2并存储2

subject.subscribe({
	next:(v) => console.log('observerB:' + v);
});

subject.next(3);
```

输出如下：

```txt
observerA: 0
observerA: 1
observerA: 2
observerB: 2
observerA: 3
observerB: 3
```

例子2：消息图标上的动态消息提示。

图标文件.html里：

```html
<mat-icon
  [matBadge]="unreadNoticeCount"
  [matBadgeHidden]="!unreadNoticeCount"
  matBadgeSize="small"
  svgIcon="notify"
></mat-icon>
```

main.service.ts里：

```typescript
unreadNoticeCount = new BehaviorSubject<number>(0);

notices = [
  { title: '111', description: '222', content: '333', status: 0 },
  { title: '444', description: '555', content: '666', status: 0 },
  { title: '777', description: '888', content: '999', status: 0 },
  { title: '123', description: '456', content: '789', status: 0 },
];

constructor() {
  const unreadNoticeCount = this.notices.reduce((count, curr) => {
    if (curr.status === 0) {
      count++;
    }
    return count;
  }, 0);
  this.unreadNoticeCount.next(unreadNoticeCount);
}
```

图标文件.ts里：

```typescript
constructor(
  private mainService: MainService,
) {
  this.mainService.unreadNoticeCount.subscribe((count) => {
    this.unreadNoticeCount = count;
  });
}
```

NotificationComponent.ts里：

```typescript
unreadNoticeCount = 0;

constructor(private mainService: MainService) {
  this.mainService.unreadNoticeCount.subscribe((count) => {
    this.unreadNoticeCount = count;
  });
}

ngAfterViewInit(): void {
  this.notices = this.mainService.notices;
}

setStatus(index: number) {
  if (this.notices[index].status === 0) {
    this.readNotice();
    this.notices[index].status = 1;
  }
}

readNotice() {
  this.mainService.unreadNoticeCount.next(--this.unreadNoticeCount);
}
```

### 表单FormGroup

eg：check password

最外层建formGroup

```html
<div [formGroup]="form">
</div>
```

同时ts里

```typescript
form: FormGroup = new FormGroup(
  {
    oldPasswordFormControl: new FormControl('', Validators.required),
    newPasswordFormControl: new FormControl('', Validators.required),
    confirmPasswordFormControl: new FormControl('', Validators.required),
  },
  this.mustMatch('newPasswordFormControl', 'confirmPasswordFormControl'),
);
```

html里，input赋上`formControlName`

```html
<input
  [type]="oldPasswordHide ? 'password' : 'text'"
  matInput
  formControlName="oldPasswordFormControl"
/>
```

password.html部分代码：

```html
<mat-form-field class="input-field lower" appearance="outline">
  <input
    [type]="oldPasswordHide ? 'password' : 'text'"
    matInput
    formControlName="oldPasswordFormControl"
    maxlength="30"
    placeholder="{{ 'main.changePassword.oldPassword' | translate }}"
    class="input-label"
  />
  <button mat-icon-button matSuffix matTooltip="visibility" (click)="oldPasswordHide = !oldPasswordHide">
    <mat-icon [svgIcon]="oldPasswordHide ? 'visibility-off' : 'visibility'"></mat-icon>
  </button>
  <!-- エラーメッセージ -->
  <mat-error *ngIf="form.get('oldPasswordFormControl')?.errors?.['required']">
    {{ 'main.changePassword.required' | translate }}
  </mat-error>
</mat-form-field>
<mat-form-field class="input-field lower" appearance="outline">
  <input
    [type]="newPasswordHide ? 'password' : 'text'"
    matInput
    formControlName="newPasswordFormControl"
    maxlength="30"
    placeholder="{{ 'main.changePassword.newPassword' | translate }}"
    class="input-label"
  />
  <button mat-icon-button matSuffix matTooltip="visibility" (click)="newPasswordHide = !newPasswordHide">
    <mat-icon [svgIcon]="newPasswordHide ? 'visibility-off' : 'visibility'"></mat-icon>
  </button>
  <!-- エラーメッセージ -->
  <mat-error *ngIf="form.get('newPasswordFormControl')?.errors?.['required']">
    {{ 'main.changePassword.required' | translate }}
  </mat-error>
</mat-form-field>
<mat-form-field class="input-field lower" appearance="outline">
  <input
    [type]="confirmPasswordHide ? 'password' : 'text'"
    matInput
    formControlName="confirmPasswordFormControl"
    maxlength="30"
    placeholder="{{ 'main.changePassword.confirmPassword' | translate }}"
    class="input-label"
  />
  <button mat-icon-button matSuffix matTooltip="visibility" (click)="confirmPasswordHide = !confirmPasswordHide">
    <mat-icon [svgIcon]="confirmPasswordHide ? 'visibility-off' : 'visibility'"></mat-icon>
  </button>
  <!-- エラーメッセージ -->
  <mat-error *ngIf="form.get('confirmPasswordFormControl')?.errors?.['required']">
    {{ 'main.changePassword.required' | translate }}
  </mat-error>
  <mat-error *ngIf="form.get('confirmPasswordFormControl')?.errors?.['mustMatch']">
    {{ 'main.changePassword.notSame' | translate }}
  </mat-error>
</mat-form-field>
<!-- 保存ボタン -->
<button mat-flat-button [disabled]="getFormValidationErrors()" class="change-pass-button" (click)="changePassword()">
  {{ 'common.button.save' | translate }}
</button>
```

password.ts部分代码：

```typescript
/**
 * 新しいパスワードと新しいパスワード（確認）を一致する必要があります。
 * @param newPasswordFormControl 新しいパスワードのフォームコントロール
 * @param confirmPasswordFormControl 新しいパスワード（確認）のフォームコントロール
 */
mustMatch(newPasswordFormControl: string, confirmPasswordFormControl: string) {
  return (formGroup: AbstractControl) => {
    const newControl = formGroup.get(newPasswordFormControl);
    const confirmControl = formGroup.get(confirmPasswordFormControl);
    if (confirmControl?.errors && !confirmControl.errors['mustMatch']) {
      return null;
    }
    if (newControl?.value !== confirmControl?.value) {
      confirmControl?.setErrors({ mustMatch: true });
    } else {
      confirmControl?.setErrors(null);
    }
    return null;
  };
}

/**
 * フォーム検証エラーを取得します。
 */
getFormValidationErrors() {
  // TODO 複数回実行
  let errorsCheck = false;
  Object.keys(this.form.controls).forEach((key) => {
    const controlErrors: ValidationErrors | null | undefined = this.form.get(key)?.errors;
    if (controlErrors != null) {
      Object.keys(controlErrors).forEach((keyError) => {
        errorsCheck = errorsCheck || controlErrors[keyError];
      });
    }
  });
  return errorsCheck;
}
```

### isObjectValueEqual

```typescript
/**
 * isObjectValueEqual
 * @param obj
 * @param copyObj
 */
isObjectValueEqual(obj: any, copyObj: any) {
  const objKeys = Object.getOwnPropertyNames(obj);
  const copyObjKeys = Object.getOwnPropertyNames(copyObj);
  if (objKeys.length !== copyObjKeys.length) {
    return false;
  }
  for (let index = 0; index < objKeys.length; index++) {
    const key = objKeys[index];
    if (typeof obj[key] === 'object' && obj[key].length) {
      if (!this.isObjectValueEqual(obj[key], copyObj[key])) {
        return false;
      }
    } else {
      if (obj[key] !== copyObj[key]) {
        return false;
      }
    }
  }
  return true;
}
```

后用第三方包：

```typescript
import { cloneDeep, isEqual, merge } from 'lodash';
```

```typescript
/**
 * データのデイープコピー
 *
 * @param data データ
 * @returns コピー結果
 */
const deepCopy = (data: any): any => cloneDeep(data);
/**
 * データのデイープ比較
 *
 * @param data1 データ1
 * @param data2 データ2
 * @returns 比較結果
 */
const deepEqual = (data1: any, data2: any): boolean => isEqual(data1, data2);
/**
 * データのデイープマージ
 *
 * @param data1 データ1
 * @param data2 データ2
 * @returns マージ結果
 */
const deepMerge = (data1: any, data2: any): any => merge(data1, data2);
```

### Reload

How to reload a component in Angular

![reload](reload.png)

## 前后台服务交互

HttpClient注入之后，可以写post，get请求

eg:

```typescript
constructor(private http: HttpClient) {}

fun() {
    this.http.post(WebapiUrl.xxxxxx, xxxxxxxxxxx);
    
    // const url....
    this.http.get(url);
}

```

### 拦载器

处理API请求的统一的处理

HttpInterceptor

### app-routing.module.ts

`onSameUrlNavigation: 'reload'` 禁用缓存

```tsx
@NgModule({
  imports: [RouterModule.forRoot(appRoutes, { preloadingStrategy: PreloadAllModules, onSameUrlNavigation: 'reload' })],
  exports: [RouterModule],
  providers: [AuthService],
})
```



## AG-Grid

### 常用属性例子

```html
<ag-grid-angular
  class="ag-grid"
  #tableItemGrid

  [gridOptions]="gridOptions"							  	← 基础
  [columnDefs]="columnDefs"
  [rowData]="rowData"
  [components]="components"									← 使用组件

  [singleClickEdit]="true"									← 单击编辑
  [stopEditingWhenCellsLoseFocus]="true"

  rowSelection="multiple"									← 不设置此参数，则row不能被单击选中
  [suppressRowClickSelection]="true"						← 同上一个参数，但此参数多用于checkbox的点击
  suppressContextMenu="true"								← 屏蔽右键菜单

  [enableCellTextSelection]="true"							← 单元格文字可以高亮选择复制

  [pagination]="true"										← 自定义分页
  [paginationPageSize]="10"
                 
  [headerHeight]="0"										← 隐藏表头

  (selectionChanged)="onSelectionChange()"
  (gridSizeChanged)="onGridSizeChanged()"
  (rowSelected)="onRowSelected($event)"
  (rowDataChanged)="onRowDataChanged()"
  (sortChanged)="onSortChanged()"
>
</ag-grid-angular>
```

常用表格写法例子

```typescript
dataSourceColumnDefs: ColDef[] = [];

ngOnInit(): void {
  this.columnDefs();
}

xxxAction = (params: EditableCallbackParams) => {
  // do something...
};

// 第一组为原生的，后两组为自己写的组件的
private columnDefs() {
  this.dataColumnDefs.push(
    {
	  headerName: 'checkBoxRegular',
	  colId: 'checkbox',
	  headerCheckboxSelection: false,
	  headerCheckboxSelectionFilteredOnly: true,
	  checkboxSelection: true,
	  width: 80,
	  minWidth: 80,
	  maxWidth: 80,
	  headerClass: 'checkBoxRegular',
	  suppressMovable: true,
	  resizable: false,
	  sortable: false,
	  suppressMenu: true,
      cellStyle: 'text-align': 'center',
	},
    {
      field: 'checkbox',
      headerName: 'checkbox',
      width: 100,
      minWidth: 50,
      cellStyle: 'text-align': 'center',
      cellRenderer: 'checkboxCellRenderer',
      cellRendererParams: { 
        action: this.xxxAction,
        color: 'Green'
   	  },
      getQuickFilterText: () => '', // 文本框过滤器
    },
    {
      field: 'input',
      headerName: 'input',
      width: 100,
      minWidth: 50,
      cellStyle: 'text-align': 'center',
      editable: true,
      cellEditor: 'inputCellRenderer',
    },
  );
    
  // GridInputComponent和GridCheckboxComponent为另写的组件
  this.components = {
    inputCellRenderer: GridInputComponent,
    checkboxCellRenderer: GridCheckboxComponent,
  };
  
  // service 里为共通的一些方法
  this.gridService.setColDef(this.dataSourceGridOptions, true, true, false);
}
```

文本框过滤器属性

```typescript
getQuickFilterText: () => '',
```

grid.service.ts里（为共通的一些方法）

```typescript
// 设置表格的一些属性，例如sortable为true为支持排序，resizable为重置表格宽度，filter为false为不支持过滤列
setColDef(options: GridOptions, resizable: boolean, sortable: boolean, filter: boolean): void {
  options.defaultColDef = { ...options.defaultColDef, resizable: resizable, sortable: sortable, filter: filter };
  options.animateRows = true;
}
```

### gridOptions

`gridOptions` 里为表格的详细信息。

eg：

```json
api: GridApi {detailGridInfoMap: {…}, destroyCalled: false, immutableService: ImmutableService, csvCreator: null, excelCreator: null, …}
columnApi: ColumnApi {columnModel: ColumnModel}
columnDefs: (8) [{…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}]
components: {selectCellRenderer: ƒ, inputCellRenderer: ƒ, checkboxCellRenderer: ƒ}
defaultColDef: {resizable: true, sortable: true, filter: false, headerValueGetter: ƒ}
localeTextFunc: (key, defaultValue) => {…}
overlayLoadingTemplate: "<span></span>"
overlayNoRowsTemplate: "<span></span>"
rowData: (2) [{…}, {…}]
singleClickEdit: true
stopEditingWhenCellsLoseFocus: true
suppressMovableColumns: true
suppressRowClickSelection: true
```

`gridOptions.columnDefs` 里是列的定义的数据。

eg：

```json
cellRenderer: "selectCellRenderer"
cellRendererParams: {$options: BehaviorSubject, filter: true, emptyOption: true, disabled: false}
cellStyle: {text-align: 'center'}
field: "inputItem"
headerName: "main.preparation.dataPipLine.label.referItem"
minWidth: 50
width: 180
```

`gridOptions.components` 里是引用的所有自定义组件。

`gridOptions.api?.getColumnDefs());` 或 `gridOptions.columnApi?.getColumnState());` 可以get到状态，同时也有set方法，resetColumnState方法。

eg：

```typescript
this.gridOptions.columnApi?.getColumnState().forEach((col) => {
  console.log('col', col);
});
```

eg：

```json
checkboxCellRenderer: class GridCheckboxComponent
inputCellRenderer: class GridInputComponent
selectCellRenderer: class GridDropdownComponent
```

`gridOptions.rowData` 里是所有行的数据，为一个array。

`gridOptions.api` 支持一些改动操作。具体看[官方文档](https://www.ag-grid.com/javascript-data-grid/grid-api/)。

### Cell Editor & Cell Render

#### 生命周期

- 组件实例化
- `agInit()` 被调用一次 (提供相应的单元格参数param).
- 组件的 GUI 将被插入网格 0 或 1 次（组件可能首先被破坏）
- `refresh()` 被调用 0...n 次（即它可能永远不会被调用，或者被调用多次）。
- 组件被销毁一次。

换句话说，组件实例化`agInit()`和销毁总是只调用一次。除非组件首先被销毁，否则组件的 GUI 通常会被渲染一次。`refresh()`可以选择多次调用。

例子：

```typescript
private params!: ICellRendererParams | any;

agInit(params: any): void {
  this.params = params;
  this.value = this.params.value;
}

refresh(params: ICellRendererParams): boolean {
  return true;
}
```

#### 例子：checkbox カラムの単一選択制御

先把选中的都选掉，然后选择的赋值，实现单一选择。

```typescript
private params!: ICellRendererParams | any;
private selection: 'multiple' | 'single' = 'multiple';
private refreshRow: boolean = false;

agInit(params: ICellRendererParams | any): void {
  this.params = params;
  this.selection = this.params.selection;

  this.refreshRow = !!this.params.columnApi
    .getAllColumns()
    .find((column: any) => column.getUserProvidedColDef()?.cellRendererParams?.controlFun);
}

onChange() {
  const field = this.params.colDef.field;
  // カラムの単一選択制御
  this.selectionControl(field);
  
  // 赋值
  this.params.node.setDataValue(field, this.value);
  // refresh row
  if (this.refreshRow) {
    this.params.api.redrawRows({ rowNodes: [this.params.node] });
  }
}

/**
 * カラムの単一選択制御
 *
 * @param field カラムのフィールド
 */
private selectionControl(field: string) {
  if (this.params.selection === 'single') {
    this.params.api.gridOptionsWrapper.gridOptions.api.forEachNode((col: any) => {
      if (col.data[field]) {
        col.setDataValue(field, false);
        this.params.api.redrawRows({ rowNodes: [col] });
      }
    });
  }
}
```

