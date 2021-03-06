假设有一个测试页面，其代码如下所示。

```html
<!DOCTYPE HTML>
<html>
<head>
  <script type="text/javascript">
    function output(resultText){
      document.getElementById('output').childNodes[0].nodeValue=resultText;
    }

    function show_confirm(){
      var confirmation=confirm("Chose an option.");
      if (confirmation==true){
        output("Confirmed.");
      }
      else{
        output("Rejected!");
      }
    }

    function show_alert(){
      alert("I'm blocking!");
      output("Alert is gone.");
    }
    function show_prompt(){
      var response = prompt("What's the best web QA tool?","Selenium");
      output(response);
    }
    function open_window(windowName){
      window.open("newWindow.html",windowName);
    }
    </script>
</head>
<body>

  <input type="button" id="btnConfirm" onclick="show_confirm()" value="Show confirm box" />
  <input type="button" id="btnAlert" onclick="show_alert()" value="Show alert" />
  <input type="button" id="btnPrompt" onclick="show_prompt()" value="Show prompt" />
  <a href="newWindow.html" id="lnkNewWindow" target="_blank">New Window Link</a>
  <input type="button" id="btnNewNamelessWindow" onclick="open_window()" value="Open Nameless Window" />
  <input type="button" id="btnNewNamedWindow" onclick="open_window('Mike')" value="Open Named Window" />

  <br />
  <span id="output">
  </span>
</body>
</html>
```

用户必须响应警告/确认弹窗，并且将焦点移到新开的弹窗上。幸运的是，Selenium 可以处理 JavaScript 弹窗。

在我们开始介绍警告/确认/提示的细节之前，有必要先了解一些共性的基础知识。警告、确认和提示提示都有下面的一些变化

|  命令   |    描述          |
| ---- | -------- |
| assertFoo(_pattern_)  | 如果模式不匹配弹窗中的文本，抛出错误        |
| assertFooPresent      | 如果没有弹窗，抛出错误                                   |
| assertFooNotPresent   | 如果存在任何弹窗，抛出错误                            |
| storeFoo(_variable_)  | 把弹窗中的文本存储在一个变量中                     |
| storeFooPresent(_variable_) | 把弹窗中的文本存储在一个变量中并返回真或假 |

当 Selenium 运行时，JavaScript 弹窗将不会出现。这是因为 JavaScript 的函数调用，在运行时被 Selenium 自己的 JavaScript 代码覆盖。然而，没看到弹窗，并不意味着不用处理它。通过调用 AssertFoo(pattern) 来处理弹窗。如果弹窗显示的断言失败了，测试会暂停并提示类似下面的错误信息：
[error] Error: There was an unexpected Confirmation! [Chose an option.]


# Alerts 警告弹窗
首先介绍警告弹窗，因为这是要处理的最简单的弹窗。首先，在浏览器中打开上面的 HTML 示例文件，单击“显示警告”弹窗按钮。请注意，当你关闭警告弹窗后，页面上会显示“Alert is gone.”。现在，在 Selenium IDE 的录制模式下，重复刚才的操作，注意文本验证命令在关闭弹窗后自动加到了脚本中。录制好的测试案例将会是下面的样子:

|  命令  |     目标  |   值      |    
| ------------- | ------------------------------------------- | ------------ |
|   open        |    /    |            |     
|  click        |    btnAlert          |              |     
|  assertAlert  |    I'm blocking!     |              |
|  verifyTextPresent  |    Alert is gone.                |              |

你可能会奇怪，并没有试图添加警告弹窗的断言。这些都是 selenium IDE 处理并关闭了警告弹窗。如果你断言的命令去掉，然后回放脚本，你会收到以下错误错误：
[error] Error: There was an unexpected Alert! [I'm blocking!]
你必须在脚本中包含一个断言来承认弹窗的存在。

如果你只是想断言警告弹窗出现了，并不关心它包含的文本是什么，这时可以使用 assertAlertPresent。他将返回真或假，当返回假时，停止测试。

# Confirmations 确认弹窗

确认的行为一样警报、assertConfirmation和assertConfirmationPresent提供警报同行一样的特征。然而,默认情况下硒会弹出一个确认时选择OK。试着记录点击显示确认boxa按钮示例页面,但是点击Cancela按钮弹出,然后断言输出文本。您的测试可能会看起来像这样:

表格4

chooseCancelOnNextConfirmation函数告诉硒都确认后应返回false。它可以通过调用chooseOkOnNextConfirmation重置。

您可能会注意到,你不能重复这个测试,因为硒抱怨说有一个未处理的确认。这是因为Selenium ide记录导致的事件的顺序点击chooseCancelOnNextConfirmation放在错误的订单(是有道理的,如果你仔细想想,硒迦南t知道百度再保险取消之前你打开一个确认)只要切换这两个命令,您的测试将会很好。

# Prompts 提示弹窗

提示的行为一样警报、assertPrompt和assertPromptPresent提供警报同行一样的特征。默认情况下,硒将等待你输入数据时,弹出的提示。试着记录点击显示prompta按钮示例页面和输入Seleniuma提示。您的测试可能会看起来像这样:

表格5

如果你选择取消的提示,您可能会注意到,answerOnNextPrompt只会显示一个空白的目标。硒(c¡)将取消和一个空白的输入提示基本上是一样的。


---
[JavaScript 和 Selenese 脚本参数](Script.md) | [目录](README.md) | [警告、弹窗和多个窗口](Alerts.md)
