http://docs.flycloud.me/docs/ELKStack/logstash/plugins/filter/grok.html
Grok是一个数据结构化工具。只需要通过简单地变量定义，我们就可以将文本格式的字符串，转换成为具体的结构化的数据。
举个例子，对于下面的日志如果要做分析，提取出关键数据如时间，请求id，耗时等等，如果单纯对下面字符串做分析，会很麻烦，如果使用Grok，就可以将下面的日志结构化，再分析就比较方便了。

2016-09-05 15:38:45.997 [catalina-exec-6] [INFO ] base.controller.BaseController [] [] KEY:93844515-1bb3-476a-9bd6-dd043ce12de11473061124949 ACTION[querybookbyname] RESP_COST[10]MS, REQ_DATA[{"action":"querybookbyname","actiondata":{
  结构化后：
timestamp=2016-09-05 15:38:45.997
threadname=catalina-exec-6
loglevel=INFO
requestid=93844515-1bb3-476a-9bd6-dd043ce12de11473061124949
action=querybookbyname
costtime=10


Grok表达式语法

USERNAME [a-zA-Z0-9._-]+
USER %{USERNAME}
第一行，用普通的正则表达式来定义一个 grok 表达式；第二行，通过打印赋值格式(sprintf format)，用前面定义好的 grok 表达式来定义另一个 grok 表达式。
grok 表达式的打印赋值格式的完整语法是下面这样的：
%{PATTERN_NAME:capture_name:data_type}
小贴士：data_type 目前只支持两个值：int 和 float。


例如：
字符串"begin 123.456 end" 会被pattern   "%{WORD} %{NUMBER:request_time:float} %{WORD}" 给捕获到，
并且把预定义的正则表达式NUMBER匹配到的123.456赋值给float型字段request_time  <<<<牛逼!
预定义的正则表达式WORD可以使用%{WORD}引用，在这个字符串中会匹配到begin和end两个单词



最后，
带你去一个调试grok表达式的好地方：http://grokdebug.herokuapp.com/