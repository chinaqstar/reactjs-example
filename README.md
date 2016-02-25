# 关于reactjs学习Demo

最近在这整理电脑的时候，发现以前学习reactjs时留下的几个代码，虽然没啥用了，但总感觉弃之可惜。

说白了就几个html中嵌入了reactjs代码，在做项目时并不是一种好的方式。但拿来学习reactjs到是挺好的。如果要开发web项目，可以试试用react-router做路由，gulp打包的一种方式。

react版本：v0.13.1

# 内容
React初识
react组件化
React定时器设置
React中ES6的使用（...props）
React事件
refs的调用
express-react-views初识

## 一、React初识
如何在html中使用reactjs代码

```
<script type="text/jsx">
      var names = ['Alice', 'Emily', 'Kate'];

      var html = [<h1>欢迎</h1>,<h5>这是一个jsx中插入html代码的例子:&lt;script type="text/jsx"&gt;</h5>,
      <h5>使用React.reder()来渲染dom标签，并将html显示出来！</h5>,
      <input type="text" value={names[1]}/>,<br></br>];

      React.render(
        <div>
        {
          html
        }
        {
           names[2]
        }
        {
          names.map(function (name) {
            return <div>Hello, {name}!</div>
          })
        }
        <br></br>
        <select >
        {
          names.map(function(name){
            return <option value={name}>{name}</option>
          })
        }
        </select>
        <br></br>
        {
          names.map(function(name) {
            return <input type="radio" name="stuName" value={name}>{name}</input>;
          })
        }
        <br></br>
        {
          names.map(function(name) {
            return <input type="checkbox" value={name}>{name}</input>
          })
        }
        </div>
        ,
        document.getElementById('example')
      );
    </script>
```

## 二、react组件化
reactjs中组件是怎样一个形式
```
<script type="text/jsx">
		var HelloTag = React.createClass({
			getInitialState: function() {
				return {hide: true};
			},
			handleClick: function() {
				this.setState({hide: !this.state.hide});
			},
			render: function() {
				var text = this.state.hide ? '' : this.props.detialCode;
				return (
					<div>
						<li onClick={this.handleClick}>
							<h4>{this.props.summaryCode}</h4>
						</li>
						<span>{text}</span>
					</div>
				);
			}
		});
		
		//React.render(<HelloTag name="123" />,document.getElementById('example'));
		
		var HelloApp = React.createClass({
			getInitialState: function() {
				return {hide: true}
			},
			handleClick: function(event) {
				// console.log(this.props.children);
				console.log(event.target);

			},
			render: function() {
				var me = this,
					detials = ["var Tag = React.createClass({...});",
						'this.props中class与for属性要在取得时候要写成：className和htmlFor。for【属于label】 === 对lable的操作，与for绑定的节点一个效果。<label for="btn1">点我相当于点【我是按钮】</label><input type="button" id="btn1" value="我是按钮"/>',
						"其实就是一个子节点的数组，可用map把子节点里的节点一个个取出来",
						'如果遇上需要与用户交互的东西（变化的dom），最好用state来更新变化'],
					desc = me.props.children.map(function(child, index) {
						// 在map里面，所以要用me ，<li onClick={me.handleClick}>
						return <HelloTag summaryCode={child} detialCode={detials[index]}></HelloTag>;
					}),
					header = [<h2>react组件化</h2>];
				return (
					<div>
						{header}
						<ol>{desc}</ol>
					</div>
					);
			}
		});
		
		React.render(
			<HelloApp >
				<span>使用this.createClass创建组件</span>
				<span>使用this.props显示</span>
				<span>使用this.children获取&lt;HelloApp&gt;下的子节点</span>
				<span>使用this.state改变状态</span>
			</HelloApp>,document.getElementById('example')); 
	</script>
```

## 三、React定时器设置
这个版本还有mixins，貌似最新版本已经没有这个东西了
```
<script type="text/jsx">
			var SetIntervalMixin = {
				componentWillMount: function() {
					this.intervals = [];
				},
				componentWillUnmount: function() {
					this.intervals.map(clearInterval);
				},
				setInterval: function() {
					this.intervals.push(setInterval.apply(null, arguments));
				}
			};

			var TickTock = React.createClass({
				mixins: [SetIntervalMixin],
				getInitialState: function() {
					return {seconds: 0};
				},
				tickWalk: function() {
					this.setState({seconds: this.state.seconds + 1});
				},
				componentDidMount: function() {
					// 全局的定时
					// setInterval(this.tickWalk, 1000);
					// TickTock的定时
					this.setInterval(this.tickWalk, 1000);
				},
				render: function() {
					return (
						<p>
							计时器（秒）：{this.state.seconds} = {Math.round(this.state.seconds/60*1000)/1000} 分 = {Math.round(this.state.seconds/3600*10000)/10000} 小时
						</p>
					);
				}
			});


			React.render(
				<div>
				<h2>经典的定时管理操作</h2>
				<h5>定时开始：setInterval(function(){'\u007b'}...{'\u007d'},1000);</h5>
				<h5>定时结束：clearInterval();</h5>
				<h2>经典的回收定时：</h2>
				<h5>var intervals = [];</h5>
				<h5>定时开始：intervals.push(setInterval.apply(null,arguments));</h5>
				<h5>定时结束：intervals.map(clearInterval);</h5>
				<TickTock />
				</div>, document.getElementById("time"));
		</script>
```
