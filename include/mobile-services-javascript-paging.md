﻿

1. 在 Visual Studio 中，打开您在完成"数据处理入门"教程后修改的项目。

2. 按 **F5** 键运行应用，然后在"插入 TodoItem"中键入文本，然后单击"保存"。

3. 重复以上步骤至少三次，因此您将在 TodoItem 表中存储三个以上的项。 

2. 在 default.js 文件中，将 **RefreshTodoItems** 方法替换为以下代码：

        var refreshTodoItems = function () {
            // Define a filtered query that returns the top 3 items.
            todoTable.where({ complete: false })
                .take(3)
                .read()
                .done(function (results) {
                    todoItems = new WinJS.Binding.List(results);
                    listItems.winControl.itemDataSource = todoItems.dataSource;
                });
        };

  	在数据绑定过程中执行此查询时，将返回未标记为已完成的前三个项。

3. 按 **F5** 键以运行应用。

  	请注意，仅显示 TodoItem 表的前三个结果。 

4. （可选）使用消息检查软件（例如浏览器开发人员工具或 [Fiddler]）来查看发送到移动服务的请求的 URI。 

   	请注意，**take(3)** 方法已转换成查询 URI 中的查询选项 **$top=3**。

5. 使用以下代码，再一次更新 **RefreshTodoItems** 方法：
            
        var refreshTodoItems = function () {
            // Define a filtered query that skips the first 3 items and 
            // then returns the next 3 items.
            todoTable.where({ complete: false })
                .skip(3)
                .take(3)
                .read()
                .done(function (results) {
                    todoItems = new WinJS.Binding.List(results);
                    listItems.winControl.itemDataSource = todoItems.dataSource;
                });
        };

   	此查询将跳过前三个结果，返回其后的三个结果。实际上这是数据的第二"页"，其页大小为三个项。

    <div class="dev-callout"><b>注意</b>
    <p>本教程将硬编码分页值传递给 <strong>Take</strong> 和 <strong>Skip</strong> 方法，因此使用的是简化的方案。在实际应用中，您可以对页导航控件或类似的 UI 使用类似于上面的查询，让用户导航到上一页和下一页。您还可以调用 <strong>includeTotalCount</strong> 方法，以获取服务器上的可用项总数以及分页的数据。</p>
    </div>

6. （可选）再次查看发送到移动服务的请求的 URI。 

   	请注意，**skip(3)** 方法已转换成查询 URI 中的查询选项 **$skip=3**。

<!-- URLs -->
[Fiddler]: http://go.microsoft.com/fwlink/?LinkID=262412
