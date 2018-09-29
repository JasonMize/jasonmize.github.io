---
layout: post
title: React Native Navigation
---


## The Overall Plan:

Add navigation to an existing React-Native app built using [Expo.](https://docs.expo.io/versions/latest/workflow/create-react-native-app){:target="_blank"}

We'll be using the [React Navigation package.](https://reactnavigation.org/docs/en/getting-started.html){:target="_blank"} 


## Step One: Installation

Install React-Navigation. 

$ `npm install --save react-navigation`


## Step Two: Configure Root
To connect the navigation system across all of the different screens/components, the root dom element needs to be setup with a list of all our components.  Then we can link back and forth between them. 

#### **App.js:**
{% highlight html %}
  import { createStackNavigator } from 'react-navigation';

  const Root = createStackNavigator(
    {
      ScreenA: ComponentForScreenA,
      ScreenB: ComponentForScreenB,
      ScreenC: ComponentForScreenC
    },
    {
      initialRouteName: 'ScreenA',
      navigationOptions: {
        headerStyle: {
          elevation: 1,
          backgroundColor: '#fff',
          height: 30,
        }
      }
    }
  );

  export default class App extends React.Component {
    render() {
      return <Root />;
    }
  }
{% endhighlight %}

Note:  Whichever component you put for 'initialRouteName' will be the component loaded when the app starts.  During development I typically make this whichever page I am working on.


## Step Three: Top Level Components
Inside each component that is a link in the menu we need to add code to handle the 'Next' and 'Back' links and the title of the current page.

#### **ComponentForScreenB.js:**
{% highlight html %}
  static navigationOptions = ({navigation}) => {
    return {
      headerLeft: (
        <HeaderBack 
          navigation={navigation}
          text={"Back"}
          nav_link={"ScreenA"}
        />
      ),
      headerTitle: (
        <HeaderTitle text={"This Page's Title"} />
      ),
      headerRight: (
        <HeaderNext 
          navigation={navigation}
          text={"Next"}
          nav_link={"ScreenC"}
        />
      ),
    };
  }
{% endhighlight %}


# Step Four: Next / Back Components
We need 2 components to handle our 'Back' and 'Next' links in the navigation menu.  You can pass variables through these links to manage state for your app.  We'll talk more about that in the next step.

#### **HeaderBack.js and HeaderNext.js:**
{% highlight html %}
  <TouchableOpacity 
    activeOpacity={.6}
    onPress={() => this.props.navigation.navigate(this.props.nav_link, {
      someVariable: this.props.someVariable,
      anotherVariable: this.props.anotherVariable
    })}
  >
    {
      this.props.text ? (
        <View>
          <Text>
            {this.props.text}
          </Text>
        </View>
      ) : null
    }
  </TouchableOpacity>
{% endhighlight %}


# Step Five: Linking To Pages
## Sending Parameters
This is how to link to another component and also pass parameters to that component.  
{% highlight html %}
  <View>
    <TouchableOpacity
      activeOpacity={.6}
      onPress={() => this.props.navigation.navigate("ComponentWeAreLinkingTo", {
        variableA: 'textInformation',
        variableB: someVariable
      })}
    >
      <Text>This is a link</Text>
    </TouchableOpacity>
  </View>
{% endhighlight %}

## Receiving Parameters
In the receiving component you access the parameters passed to you like so... 
{% highlight html %}
  componentDidMount() {
    this.handleIncomingParameter();
  }
  
  handleIncomingParameter() {
    const variableA = this.props.navigation.getParam('variableA');
    this.setState({
      variableA: variableA
    });
  }
{% endhighlight %} 

