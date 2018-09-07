个人知识体系
=============

[TOC]

## Springboot


### 配置管理

#### 增加自定义资源路径设置
```
@Configuration
public class ResourceConfig extends WebMvcConfigurerAdapter {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/upload/**").addResourceLocations("file:upload/");
    }
}
```


### JPA常用注解


#### 自增字段

```
@GeneratedValue(strategy = GenerationType.IDENTITY)
```

#### 自动创建时间,时间格式化
```
    @JsonFormat(pattern="yyyy-MM-dd HH:mm:ss",timezone = "GMT+8")
    @CreationTimestamp
```

#### 避免JPA嵌套循环问题的注解
```
@JsonIgnoreProperties("units")
```
参数为当前类在其他类中的属性名称

#### 表单CheckBox的值自动转TinyInt 值的注解
```
@Type(type = "org.hibernate.type.NumericBooleanType")
```
字段类型设置为 Boolean


#### JPA自定义查询的例子, 带KEY输出为MAP的例子

```
    @Override
    public List<?> findAllGameStateByUnitIdAndUserId(int unitId, int userId) {
        Query query = entityManager.createNativeQuery("SELECT b.*, IF(c.id > 0, 1, 0) as is_finish FROM `wx_unit_game` a" +
                " left join `wx_catalog` b on a.game_id = b.id" +
                " left join `wx_unit_game_progress` c on c.game_id = a.game_id and c.user_id = :userId" +
                " WHERE a.unit_id = :unitId and b.game_is_lock = 1");
        query.unwrap(SQLQuery.class).setResultTransformer(Transformers.ALIAS_TO_ENTITY_MAP);
        query.setParameter("userId", userId);
        query.setParameter("unitId", unitId);
        List result = query.getResultList();

        return result;
    }
```


## Maven

## Cmder

## Docker

### Image

docker pull 下载镜像

docker images 查看已下载镜像

dcoker search 搜索镜像

docker rmi 删除 -f 强制

docker ps -a 查看正在运行的镜像

### Container

#### 重启 Container
docker restart 2f29e7109f28


## MongoDB

#### 将JAR包导入Maven 的方式

```
        <dependency>
            <groupId>it.sauronsoftware</groupId>
            <artifactId>jave</artifactId>
            <version>1.0.2</version>
            <type>jar</type>
            <scope>system</scope>
            <systemPath>${basedir}/lib/jave-1.0.2.jar</systemPath>
        </dependency>
```

## Angular


### Ng-cli

https://segmentfault.com/a/1190000009771946

* ng generate class my-new-class: 新建 class

ng g cl my-new-class: 新建 class

* ng generate component my-new-component: 新建组件

ng g c my-new-component: 新建组件

* ng generate directive my-new-directive: 新建指令

ng g d my-new-directive: 新建指令

* ng generate enum my-new-enum: 新建枚举

ng g e my-new-enum: 新建枚举

* ng generate module my-new-module: 新建模块

ng g m my-new-module: 新建模块

```
--routing: boolean, 默认为 false, 表示生成一个额外包含路由信息的 AdminRoutingModule 模块，且该模块会被自动地导入到新建的模块中

--spec: boolean, 默认为 false, 表示添加 src/app/admin/admin.module.spec.ts 单元测试文件
```

* ng generate pipe my-new-pipe: 新建管道

ng g p my-new-pipe: 新建管道

* ng generate service my-new-service: 新建服务

ng g s my-new-service: 新建服务

### 自定义Select组件DEMO

```
<app-select-section [(ngModel)]="unit.sectionId" name="sectionId"></app-select-section>
```

```
<select name="selectSection" [(ngModel)]="internalValue" class="form-control custom-select" [required]="required" (blur)="onTouched($event)">
    <option *ngFor="let opt of sections" [ngValue]="opt.id">
        {{opt.sectionName}}
    </option>
</select>



import { Component, OnInit, forwardRef, OnChanges, Input, SimpleChanges } from '@angular/core';
import { SectionsService } from '../sections.service';
import { Sections } from '../sections.model';
import { ControlValueAccessor, NG_VALUE_ACCESSOR } from '@angular/forms';

@Component({
  selector: 'app-select-section',
  templateUrl: './select-section.component.html',
  styleUrls: ['./select-section.component.css'],
  providers: [
    {
      provide: NG_VALUE_ACCESSOR,
      useExisting: forwardRef(() => SelectSectionComponent),
      multi: true
    }
  ]
})
export class SelectSectionComponent implements ControlValueAccessor {
  @Input() _internalValue = "";
  @Input() required: boolean;
  
  sections: Sections[]
  public touched: boolean = false;

  constructor(
    private sectionService: SectionsService
  ) { }

  ngOnInit() {

    this.sectionService.getAll().subscribe(
      data => {
        console.log(data);
        this.sections = data;
      }
    )

  }


    public onChange: any = (_) => {  }
    public onTouched: any = () => {  }

    get internalValue() {
        return this._internalValue;
    }

    set internalValue(val: any) {
        if(val === undefined || val === null || val === "") {
            this._internalValue = "";
            this.onChange(undefined);
            return;
        }

        this._internalValue = val;
        this.onChange(this._internalValue);
    }

    writeValue(value: any) {
        if(value === undefined || value === null) {
            this.internalValue = "";
            return;
        }

        this.internalValue = value;
    }

    registerOnChange(fn: any) {
        this.onChange = fn;
    }

    registerOnTouched(fn: any) {
        let self = this;
        this.onTouched = (arg: any) => {
            self.touched = true;
            fn(arg);
        }
    }

}

```

## Linux

### Iptables

写入要开放的端口

/sbin/iptables -I INPUT -p tcp --dport 3000 -j ACCEPT

保存

/etc/rc.d/init.d/iptables save 

重启防火墙

/etc/init.d/iptables restart


查看防火墙状态

/etc/init.d/iptables status

关闭防火墙

/etc/init.d/iptables stop

永久关闭防火墙

chkconfig –level 35 iptables off

查看端口运行状态

netstat -nap | grep 3000

查看所有开发端口

netstat -ntlp

查看端口占用情况命令

lsof -i tcp:80

杀掉占用端口的进程

kill -s 9 3090

## Mysql

## Centos

## Idea

#### 快捷键

CTRL + SHIFT + ALT + T 重构

ALT + F7 查看方法引用链

CTRL + SHIFT + I 查看注释
