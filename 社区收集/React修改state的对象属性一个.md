> state = {
> 	a:{
> 		name: 'A',
> 		age: '18'
> 	}
> }

# 修改方法
> thisChangeA = （） => {
> 	let data = Object.assign({}, this.state.a, {age: 22});
> 	this.setState({
> 		a: data
> 	});
> }