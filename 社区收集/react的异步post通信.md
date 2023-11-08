
~~~js
    constructor(props) {
        super(props);
        this.state = {
            components: <View style={`font-size: 80px`}>666</View>
        };
    }

    componentWillMount() {
        window.addEventListener('message',(e) => {
            let p = 
            <View style={`font-size: 90px; color: red`}>{e.data}</View>;
            this.setState({
                components: p
            })
        });
        setTimeout(() => {
            this.renderComponent();
            window.parent!.postMessage('通信接收到的数据', '*')
        }, 3000)
    }
       render() {
        return (
            <View>
                {this.state.components}
            </View>
        );
~~~

![[../img/ali.gif]]