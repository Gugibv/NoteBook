### 一、activiti 简介

工作流：工作的一个流程，事务发展的一个业务过程

作用：实现流程自动化控制

工作流系统：具有工作流的系统

工作流系统，有哪些手段可以实现？如何实现流程的自动化管理？

> 流程化自动化管理：程序员编码实现。Activiti 可以实现业务流程变化后，程序代码不需改动。跟新的是业务流程图。

实现这个自动化：

1. 业务流程图要规范化，要遵守一套标准
2. 这个业务流程图本质是一个 xml 文件，这样就可以存入所有数据
3. 读取业务流程图的过程就是解析 xml 文件的过程
4. 读取一个业务流程图的节点就相当于解析一个 xml 结构，进一步将数据插入到 mysql 的表中，形成一条记录
5. 将所有节点都读取并存入 mysql 中
6. 后面只要读取 mysql 表中的记录，读取一条记录就相当于读一个节点
7. 业务流程的推进，后面就转化为都表中的数据，并且处理数据，结束时这一行数据就可以删除

Activiti 的运行支持，必须要有 Activiti 的25张表，主要是在流程运行过程中，记录存储一些参与流程的用户主体，组，以及流程定义的存储，流程执行时候的一些信息，以及流程的历史信息等。Activiti 的表都以 ACT_开头。 第二部分是表示表的用途的两个字母标识。 用途也和服务的  API 对应。_

- `ACT_RE_`: `RE`表示 `repository`。 这个前缀的表包含了流程定义和流程静态资源 （图片，
  规则，等等）。
- `ACT_RU_`: `RU`表示` runtime`。 这些运行时的表，包含流程实例，任务，变量，异步任务，
  等运行中的数据。` Activiti `只在流程实例执行过程中保存这些数据， 在流程结束时就会删
  除这些记录。 这样运行时表可以一直很小速度很快。
- `ACT_HI_`: `HI`表示 `history`。 这些表包含历史数据，比如历史流程实例， 变量，任务等
  等。_
- `ACT_GE_`: `GE` 表示 `general`。通用数据， 用于不同场景下。

1.1 、工作流引擎





### 二、activiti 入门体验

#### 2.1、流程定义

1. activiti-Designer使用

 	2. 绘制流程
      	3. 指定流程key
         	4. 指定任务负责人

#### 2.2、部署流程定义

部署流程定义就是要将上边绘制的图形即流程定义（.bpmn）部署在工作流程引擎 activiti 中，方法
如下：

```java
// 获取repositoryService
RepositoryService repositoryService = processEngine.getRepositoryService();
//部署对象
Deployment deployment = repositoryService.createDeployment()
			.addClasspathResource("diagram/myholiday.bpmn")// bpmn文件
			.addClasspathResource("diagram/myholiday.png") // 图片文件
			.name("请假申请流程")
			.deploy();
System.out.println("流程部署id:" + deployment.getId());
System.out.println("流程部署名称:" + deployment.getName());
```

执行此操作后 activiti 会将上边代码中指定的 bpm 文件和图片文件保存在 activiti 数据库。

#### 2.3、启动一个流程实例

流程定义部署在 activiti 后就可以通过工作流管理业务流程了，也就是说上边部署的请假申请流
程可以使用了。

针对该流程，启动一个流程表示发起一个新的请假申请单，这就相当于 java 类与 java 对象的关
系，类定义好后需要 new 创建一个对象使用，当然可以 new 多个对象。对于请假申请流程，张三发
起一个请假申请单需要启动一个流程实例，请假申请单发起一个请假单也需要启动一个流程实例。

代码如下：

```java
// 启动一个流程实例
@Test
public void startProcessInstance() {
	// 获取RunTimeService
	RuntimeService runtimeService = processEngine.getRuntimeService();
	// 根据流程定义key启动流程
	ProcessInstance processInstance = runtimeService
				.startProcessInstanceByKey("myholiday01");
	System.out.println(" 流程定义 id ：" + processInstance.getProcessDefinitionId());
	System.out.println("  流程实例id：" + processInstance.getId());
	System.out.println(" 当前活动 Id ：" + processInstance.getActivityId());
}
```

流程定义与流程实例区别：

```tex
流程定义                  流程部署
   bpmn----------------->activiti 的三张表
流程实例
   流程定义好比java的一个类
   流程实例好比java的一个实例对象，一个流程定义可以对应多个流程实例
```

`act_re_deployment`   部署信息
`act_re_procdef`         流程定义的一些信息
`act_ge_bytearray`     pmpmn  文件及 png文件

说明：`act_re_deployment `和 `act_re_procdef 一`对多关系，一次部署在流程部署表生成一条记录，但一次部署可以部署多个流程定义，每个流程定义在流程定义表生成一条记录。每一个流程定义在`act_ge_bytearray` 会存在两个资源记录，`bpmn`和 `png`。
建议：一次部署一个流程，这样部署表和流程定义表是一对一有关系，方便读取流程部署及流程定义信息。

#### 2.4、任务查询

流程启动后，各各任务的负责人就可以查询自己当前需要处理的任务，查询出来的任务都是该用户
的待办任务。

```java
// 查询当前个人待执行的任务act_ru_task
@Test
public void findPersonalTaskList() {
	// 任务负责人
	String assignee = "zhangsan";
	// 创建TaskService
	TaskService taskService = processEngine.getTaskService();
	List<Task> list = taskService.createTaskQuery()
				.processDefinitionKey("myholiday01")
				.taskAssignee(assignee)//只查询该任务负责人的任务
				.list();
	for (Task task : list) {
		System.out.println(" 流 程 实 例 id ：" + task.getProcessInstanceId());
		System.out.println("任务id："    + task.getId());
		System.out.println("任务负责人：" + task.getAssignee());
		System.out.println("任务名称："   + task.getName());
	}
}
```

#### 2.5、任务处理

任务负责人查询待办任务，选择任务进行处理，完成任务。

```java
// 完成任务act_ru_task
@Test
public void completTask() {
	//任务id
	String taskId = "8305";
	// 创建TaskService
	TaskService taskService = processEngine.getTaskService();
	//完成任务
	taskService.complete(taskId);
	System.out.println("完成任务id="+taskId);
}
```

#### 2.6、判断流程结束

```java
	//判断流程是否结束，查询正在执行的执行对象表 act_ru_execution   
	ProcessInstance rpi = processEngine.getRuntimeService()//
    	.createProcessInstanceQuery()//创建流程实例查询对象
    	.processInstanceId(pi.getId())
    	.singleResult();
 	

	if(rpi==null){//说明流程实例结束了  
 
    }
```

#### 2.7、流程变量

流程变量在整个工作流中扮演非常重要的作用，例如：请假流程中有请假天数，请假原因等。流程变量的作用范围只对应一个流程实例，流程实例结束完成后流程变量还保存在数据库中（存放到流程变量的历史表中）

通过流程变量控制流程的走向

### 附件一、相关表

```sql
-- ReponsitoryService
select *  from  act_ge_bytearray ;  		 -- 二进制文件表
select *  from  act_re_deployment ;			 -- 流程部署表
select *  from  act_re_procdef; 			 -- 流程定义表
select *  from  act_ge_property;             -- 工作流的ID算法和版本信息表

-- RuntimeTaskService
select *  from  act_ru_execution;  			 -- 流程启动一次只要没有执行完，就会有一条记录
select *  from  act_ru_task;       			 -- 可能有多条数据
select *  from  act_ru_variable;    		 -- 记录流程运行时的流程变量
select *  from  act_ru_identitylink;		 -- 存放流程办理人的信息

-- HistoryService
select *  from  act_hi_procinst;    		  -- 历史流程实例
select *  from  act_hi_taskinst;    		  -- 历史任务实例
select *  from  act_hi_actinst;     		  -- 历史活动节点表
select *  from  act_hi_varinst;     		  -- 历史流程变量历史表
select *  from  act_hi_identitylink; 		  -- 历史办理人表
select *  from  act_hi_comment;   		      -- 批准表
select *  from  act_hi_attachment;  		  -- 附件表

-- IdentityService
select * from act_id_group;  				   -- 角色
select * from act_id_membership; 			   -- 用户和角色之间的关系
select * from act_id_info;  			       -- 用户的详细信息
select * from act_id_user;  				   -- 用户表
```

说明：

1. `act_ru_execution` 表字段 `BUSINESS_KEY_` 业务ID 和 流程执行实例相绑定。

   

   