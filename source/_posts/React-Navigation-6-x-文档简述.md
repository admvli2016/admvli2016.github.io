---
title: React Navigation 6.x 文档简述
date: 2021-12-24 10:57:06
tags: [React Native,React Navigation]
categories: 移动端开发
typora-copy-images-to: upload
---

## React Navigation 入门

1.1 安装 `@react-navigation/native`

```shell
# 安装导航库
npm install @react-navigation/native
```

报错：

![](https://gitee.com/admvli2016/pictures/raw/master/img/%E6%88%AA%E5%9B%BE%20(14).png)

<!--more-->

版本要求

- react-native >= 0.63.0
- typescript>= 4.1.0

typescript 版本太低，升级 typescript 版本就行

```shell
npm uninstall typescript
npm install --save-dev typescript
```

再次安装

```shell
npm install @react-navigation/native
```



1.2 安装其他相关的依赖

```shell
# 安装其他相关的依赖
npm install react-native-screens react-native-safe-area-context
```

iOS 从 React Native 0.60 及更高版本开始，不需要运行 `react-native link`
如果使用的是 Mac 开发 iOS 应用，则需要 pod-install（通过Cocoapods）以完成链接。

```shell
npx pod-install ios
```

Android 需要一个额外的配置步骤：编辑 `MainActivity.java`

文件位置： `android/app/src/main/java/<your package name>/MainActivity.java`

将以下代码添加至 `MainActivity` 类的内部:

```java
@Override
protected void onCreate(Bundle savedInstanceState) {  
    super.onCreate(null);
}
```

并下面这一句放在文件开头位置

```java
import android.os.Bundle;
```



1.3 在项目中使用 `NavigationContainer`

```typescript
// 将下列语句放到 app.tsx 中
import * as React from 'react';
import { NavigationContainer } from '@react-navigation/native';

export default function App() {
  return (    
      <NavigationContainer>{/* Rest of your app code */}</NavigationContainer>  
  );
}
```



1.4 安装 `@react-navigation/native-stack`

```shell
# 安装 @react-navigation/native-stack
npm install @react-navigation/native-stack
```



1.5 在项目中使用 `createNativeStackNavigator` 、 `Stack.Navigator` 、`Stack.Screen`

```typescript
// In App.js in a new project

import * as React from 'react';
import { View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
```



1.6 在项目中使用 `initialRouteName` 配置堆栈的初始路由

```typescript
import * as React from 'react';
import { View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
    </View>
  );
}

function DetailsScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Details">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
```

修改 `initialRouteName` 后需要重新加载项目，快速刷新对修改 `initialRouteName` 不起作用（不会生效）

需要说明的是，component 属性接收的是组件而不是一个 render() 方法。所以不要给它传递一个内联函数（比如 component={()=> }）,否则你的组件在被卸载并再次加载的时候将会丢失所有的 state。



1.7 设置 `option` 参数

导航器中的每一个路由页面都可以设置一些参数，例如 `title`

```html
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={{ title: 'Overview' }}/>
```

如果要传递一些额外的属性，还可以使用一个 render 函数替换 component 属性，如下所示

```html
<Stack.Screen name="Home">
  {props => <HomeScreen {...props} extraData={someData} />}
</Stack.Screen>
```

这种方式被称之为渲染回调

> 注意：默认情况下，React Navigation 会对屏幕组件应用优化措施以防止不必要的渲染。使用渲染回调将应用不了这些优化。因此，如果您使用渲染回调，则需要确保对屏幕组件使用 `React.memo` 或 `React.PureComponent` 以避免性能问题。



## 路由栈跳转

Web 路由的跳转方式

```html
// 方式一
<a href="details.html">Go to Details</a>

// 方式二
<a
  onClick={() => {
    window.location.href = 'details.html';
  }}
>
  Go to Details
</a>
```

2.1 跳转到一个新页面

```typescript
import * as React from 'react';
import { Button, View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

function HomeScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
      <Button
        title="Go to Details"
        onPress={() => navigation.navigate('Details')}
      />
    </View>
  );
}

// ... other code from the previous section
```

- navigation ：在堆栈导航器中，navigation 属性会被传递到每一个 screen 组件中。
- navigate() ： 我们调用 navigate 方法来实现页面的跳转。



2.2 多次跳转到同一路由

```typescript
function DetailsScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
      <Button
        title="Go to Details... again"
        onPress={() => navigation.navigate('Details')}
      />
    </View>
  );
}
```

运行上面的代码，当点击 Go to Details... again 时，什么事情都不会发生，这是因为我们已经在 Details 页面了。

现在假设一下，如果我们想要添加另一个 Details 页面呢？这是一个很常见的场景，在这个场景中我们想在同一个 Details 页面展示不同的数据。为了实现这个 ，我们可以用 push() 方法来替代 navigate() 方法，push() 方法可以直接向堆栈中添加新的路由而无视当前导航历史。

```html
<Button
  title="Go to Details... again"
  onPress={() => navigation.push('Details')}
/>
```



2.3 路由返回

堆栈导航器提供的导航头默认包含一个返回按钮，点击按钮可以返回到上一个页面，如果导航堆栈中只有一个页面，也就是说并没有可以返回的页面的时候，这个回退按钮就不显示。

如果我们使用的是自定义的导航头，可以使用 `navigation.goBack()` 方法来实现路由返回，如下所示。

```typescript
function DetailsScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
      <Button
        title="Go to Details... again"
        onPress={() => navigation.push('Details')}
      />
      <Button title="Go to Home" onPress={() => navigation.navigate('Home')} />
      <Button title="Go back" onPress={() => navigation.goBack()} />
    </View>
  );
}
```

另一个常见的需求就是返回到多个页面之前。比如，如果你的堆栈中有多个页面，你想依次移除页面然后返回到第一个页面上。在上面的场景中，我们想要返回Home 页面，所以我们可以使用 `navigate('Home')`。

另一种方法就是 `navigation.popToTop()`，这个方法将会返回到堆栈中的第一个页面上（初始页面），如下所示。

```typescript
function DetailsScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
      <Button
        title="Go to Details... again"
        onPress={() => navigation.push('Details')}
      />
      <Button title="Go to Home" onPress={() => navigation.navigate('Home')} />
      <Button title="Go back" onPress={() => navigation.goBack()} />
      <Button
        title="Go back to first screen in stack"
        onPress={() => navigation.popToTop()}
      />
    </View>
  );
}
```



## 路由传参

3.1 基本用法

在页面跳转的过程中，免不了会进行数据的传递。在React Navigation中，路由之间传递参数主要有两种方式：

- 将参数封装成一个对象，然后将这个对象作为 `navigation.navigate()`、`navigation.push()` 方法的第二个参数，从而实现路由跳转参数的传递。
- 使用 `route.params`  读取传递过来的参数。

不管是哪种方式，我们建议对需要传递的数据进行序列化？？？，如下所示。

```typescript
import * as React from 'react';
import { Text, View, Button } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

function HomeScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
      <Button
        title="Go to Details"
        onPress={() => {
          /* 1. Navigate to the Details route with params */
          navigation.navigate('Details', {
            itemId: 86,
            otherParam: 'anything you want here',
          });
        }}
      />
    </View>
  );
}

function DetailsScreen({ route, navigation }) {
  /* 2. Get the param */
  const {itemId, otherParam} = route.params;
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
      <Text>itemId: {JSON.stringify(itemId)}</Text>
      <Text>otherParam: {JSON.stringify(otherParam)}</Text>
      <Button
        title="Go to Details... again"
        onPress={() =>
          navigation.push('Details', {
            itemId: Math.floor(Math.random() * 100),
          })
        }
      />
      <Button title="Go to Home" onPress={() => navigation.navigate('Home')} />
      <Button title="Go back" onPress={() => navigation.goBack()} />
    </View>
  );
}

const Stack = createStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```



3.2 更新参数

在路由到的页面上也可以更新这些参数，`navigation.setParams()`方法就是用来更新这些参数的。

```js
navigation.setParams({
  query: 'someText',
});
```

此方法必须要在路由到的页面组件挂载之后再进行调用

如下所示

```typescript
function DetailsScreen({route, navigation }:any) {
  // 获取路由参数
  const {itemId, otherParam} = route.params;
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'space-around' }}>
      <Text>Details Screen</Text>
      <Text>itemId: {JSON.stringify(itemId)}</Text>
      <Text>otherParam: {JSON.stringify(otherParam)}</Text>
      <Button title="Set Params" onPress={()=>{
        navigation.setParams({
          itemId: 'someText'
        });
      }}></Button>
      <Button title="Go to Details ... again" onPress={() => navigation.push('Details', {
        itemId: Math.ceil(Math.random()*100)
      })} />
      <Button title="Go to Home" onPress={() => navigation.navigate('Home')} />
      <Button title="Go Back" onPress={() => navigation.goBack()} />
      <Button title="Go Back to first screen in stack" onPress={() => navigation.popToTop()}></Button>
    </View>
  );
}
```



3.3 初始化参数

您还可以将一些初始参数传递给屏幕。如果您在导航到此屏幕时未指定任何参数，则将使用初始参数。它们也可以与您传递的任何参数浅合并。可以使用 `initialParams` 属性指定初始参数：

```html
<Stack.Screen
  name="Details"
  component={DetailsScreen}
  initialParams={{ itemId: 42 }}
/>
```



3.4 返回参数给上一个路由

我们将数据通过参数的方式传递给一个新的路由页面，也可以将数据回传给先前的路由页面。例如，有一个路由页面，该页面有一个创建帖子的按钮，这个按钮可以打开一个新的页面来创建帖子，在创建完帖子之后，需要将帖子的一些数据回传给先前的页面。

对于这种需求，我们可以使用 `navigation.navigate()` 来实现。如果路由在堆栈中已经存在了，他们看起来就像 goBack 一样，只需要将数据绑定到 `navigation.navigate()` 的 params 中回传给上一页即可，代码如下

```typescript
import * as React from 'react';
import { Text, TextInput, View, Button } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

function HomeScreen({ navigation, route }) {
  React.useEffect(() => {
    if (route.params?.post) {
      // Post updated, do something with `route.params.post`
      // For example, send the post to the server
    }
  }, [route.params?.post]);

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Button
        title="Create post"
        onPress={() => navigation.navigate('CreatePost')}
      />
      <Text style={{ margin: 10 }}>Post: {route.params?.post}</Text>
    </View>
  );
}

function CreatePostScreen({ navigation, route }) {
  const [postText, setPostText] = React.useState('');

  return (
    <View>
      <TextInput
        multiline
        placeholder="What's on your mind?"
        style={{ height: 200, padding: 10, backgroundColor: 'white' }}
        value={postText}
        onChangeText={setPostText}
      />
      <Button
        title="Done"
        onPress={() => {
          // Pass params back to home screen
          navigation.navigate('Home', { post: postText });
        }}
      />
    </View>
  );
}

const Stack = createStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator mode="modal">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="CreatePost" component={CreatePostScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

当点击 Done 按钮后，Home 页面的 route.params 将会被更新并刷新文本。



3.5 将参数传递给嵌套的导航器

如果涉及有嵌套导航器的场景，则需要以稍微不同的方式传递参数。例如，假设在 Acount 页面嵌套了一个 Settings 页面，并且希望将参数传递到该导航器中的 Settings 页面中，那么可以使用下面的方式。

```js
navigation.navigate('Account', {
  screen: 'Settings',
  params: { user: 'jane' },
});
```



3.6 参数中应该包含什么

了解 params 中应该包含什么样的数据很重要。参数就像屏幕的选项。它们应该只包含用于配置屏幕中显示内容的信息。避免传递将显示在屏幕本身上的完整数据（例如传递用户 ID 而不是用户对象）。还要避免传递被多个屏幕使用的数据，这些数据应该放在全局存储中。

你也可以把路由对象想象成一个 URL。如果您的屏幕有一个 URL，那么 URL 中应该包含什么？参数不应包含您认为不应出现在 URL 中的数据。这通常意味着您应该使用尽可能少的数据来确定屏幕是什么。想想访问一个购物网站，当您看到产品列表时，URL 通常包含类别名称、排序类型、过滤字段等，它不包含在屏幕上实际显示的产品列表。

例如，假设您有一个 Profile 屏幕。导航到它时，您可能会想在参数中传递用户对象：

```js
// Don't do this
navigation.navigate('Profile', {
  user: {
    id: 'jane',
    firstName: 'Jane',
    lastName: 'Done',
    age: 25,
  },
});
```

这看起来很方便，让您的 route.params.user 无需任何额外工作即可访问用户对象。

然而，这是一种反模式。诸如用户对象之类的数据应该在您的全局存储中而不是在导航状态中。否则，您会在多个地方复制相同的数据。这可能会导致错误，例如即使用户对象在导航后发生了更改，Profile 屏幕也会显示过时的数据。

通过深层链接或者在 Web 上链接到屏幕时也会出现问题，因为：

- URL 是屏幕的表示，因此还需要包含参数，即完整的用户对象，这会使 URL 非常长且不可读
- 由于用户对象在 URL 中，因此有可能传递一个随机用户对象来代表一个不存在的用户，或者在配置文件中有不正确的数据
- 如果用户对象没有被传递，或者格式不正确，这可能会导致崩溃，因为屏幕不知道该如何处理它

更好的方法是在 params 中只传递用户的 ID：

```js
navigation.navigate('Profile', { userId: 'jane' });
```

现在，您可以使用传递的 userId 从您的全局存储中获取用户。这消除了许多问题，例如过时的数据或有问题的 URL。

参数中应该包含的内容的一些示例：

- 用户 ID、商品 ID 等 ID，例如 `navigation.navigate('Profile', { userId: 'Jane' })`
- 当您有项目列表时，用于排序、过滤数据等的参数，例如 `navigation.navigate('Feeds', { sortBy: 'latest' })`
- 用于分页的时间戳、页码或光标，例如 `navigation.navigate('Chat', { beforeTime: 1603897152675 })`
- 填充屏幕上的输入以组成某些东西的数据，例如 n`avigation.navigate('ComposeTweet', { title: 'Hello world!' })`

本质上，在 params 中传递识别屏幕所需的最少数据，在很多情况下，这只是意味着传递对象的 ID 而不是传递完整的对象。

将您的应用程序数据与导航状态分开。



## 导航栏配置

4.1 设置导航栏标题

React Navigation 的 Screen 组件有一个 options 属性，这个属性包含了很多可配置的选项，例如设置导航栏的标题，如下所示。

```typescript
function StackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Home"
        component={HomeScreen}
        options={{ title: 'My home' }}
      />
    </Stack.Navigator>
  );
}
```



4.2 在标题中使用参数

 为了在标题中使用参数，我们可以把 options 属性定义为一个返回配置对象的方法。即我们可以将 options 定义为一个方法，React Navigation 调用它并为其传入两个可供使用的参数`{navigation,route}` 即可，如下所示。

```typescript
function HomeScreen({ navigation, route }:any) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Button 
        title="Set Profile Name" 
        onPress={() => { 
          navigation.navigate('Profile', { name: "我来即我见"})
        }} />
    </View>
  );
}

function ProfileScreen({ navigation, route }:any) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Profile Screen</Text>
    </View>
  )
}

function StackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Home"
        component={HomeScreen}
        options={{ title: 'My home' }}
      />
      <Stack.Screen
        name="Profile"
        component={ProfileScreen}
        options={({ route }:any) => ({ title: route.params.name })}
      />
    </Stack.Navigator>
  );
}
```

- navigation：屏幕组件的 navigation 属性
- route：屏幕组件的 route 属性



4.3 使用 setOptions 更新 options

有时候，我们需要在一个已加载的屏幕组件上更新它的 options 配置，对于这样的需求，我们可以使用 `navigation.setOptions()` 来实现。

```html
<!-- Inside of render() of React class -->
<Button
  title="Update the title"
  onPress={() => navigation.setOptions({ title: 'Updated!' })}/>
```



4.4 设置导航栏的样式

React Navigation支持自定义导航头的样式，我们可以使用如下三个样式属性 headerStyle、headerTintColor 和 headerTitleStyle，含义如下：

- headerStyle：用于设置包裹住导航头的视图样式，比如为导航栏设置背景色。
- headerTintColor：回退按钮和title都是使用这个属性来设置他们的颜色。
- headerTitleStyle：如果需要自定义字体、字体粗细和其他文本样式，可以使用属性。

```typescript
function StackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Home"
        component={HomeScreen}
        options={{
          title: 'My home',
          headerStyle: {
            backgroundColor: '#f4511e',
          },
          headerTintColor: '#fff',
          headerTitleStyle: {
            fontWeight: 'bold',
          },
        }}
      />
    </Stack.Navigator>
  );
}
```

自定义导航栏样式的时候，有以下几点需要注意：

- 在IOS上，状态栏文本和图标是黑色的，所以你将背景色设置为暗色的话可能看起来不是很好。
- 以上的设置仅仅只在 Home 页面有效，当我们跳转到 Details 页面的时候，依然会显示默认的颜色。



4.5 与其他屏幕组件共享 options 参数

有时候，我们需要多个页面设置相同的导航头样式。那么，我们可以使用堆栈导航器的 `screenOptions `属性。

```typescript
function StackScreen() {
  return (
    <Stack.Navigator
      screenOptions={{
        headerStyle: {
          backgroundColor: '#f4511e',
        },
        headerTintColor: '#fff',
        headerTitleStyle: {
          fontWeight: 'bold',
        },
      }} >
      <Stack.Screen
        name="Home"
        component={HomeScreen}
        options={{ title: 'My home' }} />
    </Stack.Navigator>
  );
}
```

在上面的例子中，任何属于 StackScreen 的页面都将使用相同样式的导航头。



4.6 自定义组件替换 title 属性

有时候，我们想对 title 做更多的设置，比如使用一张图片来替换 title 的文字，或者将 title 放进一个按钮中，这些场景中我们完全可以使用自定义的组件来覆盖 title，如下所示。

```typescript
function LogoTitle() {
  return (
    <Image
      style={{ width: 50, height: 50 }}
      source={require('@expo/snack-static/react-native-logo.png')}
    />
  );
}

function StackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Home"
        component={HomeScreen}
        options={{ headerTitle: props => <LogoTitle {...props} /> }}/>
    </Stack.Navigator>
  );
}
```



## 导航栏按钮

5.1 为导航栏添加按钮

通常，我们可以使用导航头的按钮来实现路由操作。比如，我们点击导航栏左边的按钮可以返回上一个路由，点击导航头的右边可以进行其他操作，如下所示。

```typescript
import { Alert } from 'react-native';

function StackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Home"
        component={HomeScreen}
        options={{
          headerTitle: props => <LogoTitle {...props} />,
          headerRight: () => (
            <Button
              onPress={() => Alert.alert('This is a button!')}
              title="Info"
              color="#fff"
            />
          ),
        }}
      />
    </Stack.Navigator>
  );
}
```



5.2 导航头与屏幕组件交互

只需要使用 `navigation.setOptions()` 即可实现按钮与屏幕组件的交互，可以使用 `navigation.setOptions()` 来访问 screen 组件的属性如 props，state 和 context 等。

```typescript
function StackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Home"
        component={HomeScreen}
        options={({ navigation, route }) => ({
          headerTitle: props => <LogoTitle {...props} />,
        })}
      />
    </Stack.Navigator>
  );
}

function HomeScreen({ navigation }) {
  const [count, setCount] = React.useState(0);

  React.useLayoutEffect(() => {
    navigation.setOptions({
      headerRight: () => (
        <Button onPress={() => setCount(c => c + 1)} title="Update count" />
      ),
    });
  }, [navigation]);

  return <Text>Count: {count}</Text>;
}
```



5.3 自定义返回按钮

createStackNavigator 提供了平台默认的返回按钮，在 iOS 平台上，在按钮的旁边会有一个标签，这个标签会显示上一页标题的缩写，上一页没有标题就显示一个Back字样。

开发者可以通过设置 `headerBackTitle` 来改变标签文字，设置 `headerBackTitleStyle` 改变标签的样式，自定页返回按钮图片可以使用 `headerBackImageSource`



5.4 重写返回按钮

```html
<Screen
  name="Home"
  component={HomeScreen}
  options={{
    headerLeft: (props) => (
      <Button title="返回" {...props} onPress={()=>Alert.alert('返回上个页面')} />
    )
  }}
/>;
```



## 导航器的嵌套

导航嵌套指的是一个导航器的导航页中又包含了另一个导航器，例如：

```typescript
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs'; 

function FeedScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Feed Screen</Text>
    </View>
  )
}

function MessageScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Message Screen</Text>
    </View>
  )
}

const Tab = createBottomTabNavigator();

function TabsScreen() {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Feed" component={FeedScreen} />
      <Tab.Screen name="Message" component={MessageScreen}/>
    </Tab.Navigator>
  )
}

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={Home} />
        <Stack.Screen name="Tabs" component={TabsScreen} options={{ headerShown: false }}/>

      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

在上述的例子中，Home 组件包含了一个 Tab 导航器。同时，这个 Home 组件也是 Stack 导航器中的一个导航页，从而形成了一个导航嵌套的场景。

导航器的嵌套其实跟我们通常使用的组件的嵌套类似，一般情况下，我们会嵌套多个导航器。



6.1 如何实现导航器嵌套

实现导航器嵌套时，有以下几点需要特别注意：

（1）每个导航器管理它自己的导航栈

比如，当你在一个被嵌套的堆栈导航器中点击返回按钮的时候，它会返回到本导航器（也就是被嵌套的堆栈导航器）导航历史中的上一页，而不是返回到上级导航器中。

（2）每个导航器管理它自己的 options

比如，当你在被嵌套的子导航器的屏幕中设置 title 选项时，不会影响父导航器中显示的标题。

如果你想要在堆栈导航器中显示被嵌套的选项卡导航器处于活跃状态的屏幕的 title，请参考指南  [screen options with nested navigators](https://reactnavigation.org/docs/screen-options-resolution#setting-parent-screen-options-based-on-child-navigators-state)

（3）导航器中的每个屏幕管理它自己的参数

任何传递给在嵌套导航器中的屏幕的 params 都在此屏幕的 route prop 中，并且不能被来自父导航器与子导航器中的屏幕访问

如果你需要在子导航器屏幕中访问父导航器屏幕中的参数，你可以使用 [React Context](https://reactjs.org/docs/context.html) 将参数暴露给子导航器屏幕

（4）导航动作将会被当前导航器处理，如果不能处理，它将会冒泡

比如，你在被嵌套导航器的屏幕中调用 `navigation.goBack()`，只有你已经是当前导航器的第一个屏幕时，才会在父导航器中返回到上一页。其他操作诸如 navigate 也是类似的，导航将会在嵌套导航器中发生，如果嵌套导航器不能够处理，那么父导航器将会尝试处理它。在上面的例子中，当我们调用 `navigation.navigate('Message')` ，在 Feed 屏幕当中，被嵌套的 Tab 导航器将会处理它；但是如果你调用 `navigation.navigate('Home')`，父导航器将会处理它

（5）导航器的一些特定方法在子导航器中同样可用

在 Drawer 导航器中嵌套了一个 Stack 导航器，那么 Drawer 导航器的 openDrawer、closeDrawer 方法在被嵌套的 Stack 导航器的 navigation 属性中依然是可用的。但是如果 Stack 导航器没有嵌套在 Drawer 导航器中，那么这些方法是不可访问的。

同样，如果你在 Stack 导航器中嵌套了一个 Tab 导航器，那么 Tab 导航器的 navigation 属性中会新得到 push 和 replace 这两个方法。

如果你需要从父级分发动作给被嵌套的子导航器，你可以使用 `navigation.dispatch()`

```js
navigation.dispatch(DrawerActions.toggleDrawer());
```

（6）被嵌套的导航器不会响应父级导航器的事件

如果 Stack 导航器被嵌套在 Tab 导航器中，那么 Stack 导航器的页面不会响应由父 Tab 导航器触发的事件，比如我们使用 `navigation.addListener()` 绑定的 `tabPress` 事件。为了能够响应父级导航器的事件，我们可以使用 `navigation.getParent()` 来监听父级导航器的事件。

```js
const unsubscribe = navigation.getParent().addListener('tabPress', (e) => {
  // Do something
});
```

（7）父级导航器先于子导航器被渲染

对于上面的这句话，我们怎么理解呢？在 Drawer 导航器中嵌套了一个 Stack 导航器，你会看到抽屉的效果先被渲染出来，接着才是渲染 Stack 导航器的头部，但是如果将 Drawer 导航器嵌套在 Stack 导航器中，那么则会先渲染Stack 导航器的头部再渲染抽屉效果。因此，我们可以根据这一现象来确认我们需要选择哪种导航器的嵌套。

在你的 App 里面，你可能会根据你想要的行为使用这些模式

- Tab 导航器嵌套在 Stack 导航器的初始屏幕中 - 当你 push 新屏幕时将会覆盖 Tab bar
- Drawer 导航器嵌套在 Stack 导航器的初始屏幕中，初始屏幕的 Stack header 隐藏 - Drawer 只能从 Stack 的第一个屏幕中打开
- Stack 导航器被嵌套在 Drawer 导航器的每个屏幕中 - Drawer 出现在 Stack header 上方
- Stack 导航器被嵌套在 Tab 导航器的每个屏幕中 - Tab bar 始终可见。 通常按下 Tab，将会调用 `navigation.popToTop()`



6.2 嵌套路由的跳转

我们先来看一下下面这段代码。

```typescript
function Root() {
  return (
    <Drawer.Navigator>
      <Drawer.Screen name="Home" component={Home} />
      <Drawer.Screen name="Profile" component={Profile} />
      <Stack.Screen name="Settings" component={Settings} />
    </Drawer.Navigator>
  );
}

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen
          name="Root"
          component={Root}
          options={{ headerShown: false }}
        />
        <Stack.Screen name="Feed" component={Settings} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

假如，现在想从 Feed 页面中跳转到 Root 页面，那么我们可以使用下面的方式

```js
navigation.navigate('Root');
```

它生效了，并且显示 Root 组件内部的初始屏幕，即 Home，不过，有时你可能希望显示自己指定的页面，为了实现这个功能，可以在参数中携带页面的名字，如下所示。

```js
navigation.navigate('Root',{screen: 'Profile'}});
```

此时，将呈现 Proilfe 屏幕

这可能看起来与以前的用于嵌套屏幕的导航方式大不相同。不同之处在于，在之前的版本中，所有配置都是静态的，因此 React Navigation 可以通过递归嵌套配置来静态查找所有导航器及其屏幕。但是现在通过动态配置，React Navigation 不知道哪些屏幕可用以及在哪里，直到包含屏幕的导航器呈现。通常，屏幕在您导航到它之前不会呈现其内容，因此尚未呈现的导航器配置尚不可用。这使得有必要指定您要导航到的层次结构。这也是为什么您应该尽可能少地嵌套导航器以保持代码简洁。

6.2.1 传递参数给被嵌套导航中的屏幕

```js
navigation.navigate('Root', {  
    screen: 'Profile',  
    params: { user: 'jane' }
});
```

如果导航器已经被渲染，导航到另一个屏幕将在堆栈导航器中压入一个新屏幕

对于深度嵌套的屏幕，可以采用类似的方法。请注意，navigate 此处的第二个参数是 params，因此您可以执行以下操作：

```js
navigation.navigate('Root', {
  screen: 'Settings',
  params: {
    screen: 'Sound',
    params: {
      screen: 'Media',
    },
  },
});
```

在上述情况下，您将导航到 Media 屏幕，它位于嵌套在 Sound 屏幕内的导航器中，而导航器位于嵌套在 Settings 屏幕内的导航器中。



6.2.2 显示在导航器中定义的初始屏幕  ？？？（自己验证并没有作用）

默认情况下，当您在嵌套导航器中导航屏幕时，指定的屏幕将用作初始屏幕，并且导航器上的初始路由 prop 将被忽略。这种行为不同于 React Navigation 4。

如果您需要渲染导航器中指定的初始路由，您可以通过设置 `initial: false` 禁用使用指定屏幕作为初始屏幕的行为：

```js
navigation.navigate('Root', {  
    screen: 'Settings',  
    initial: false
});
```

这将会影响到按下后退按钮时发生的情况。当有初始屏幕时，后退按钮会将用户带到那里。？？？



6.3 嵌套多个导航器

嵌套多个导航器（例如堆栈、抽屉或选项卡）有时很有用。

当嵌套多个堆栈、抽屉或底部选项卡导航器时，将显示来自子导航器和父导航器的标题。但是，通常更希望在子导航器中显示标题并在父导航器的屏幕中隐藏标题。

为此，您可以使用该 `headerShown: false` 选项隐藏包含导航器的屏幕中的标题。

例如：

```typescript
function Home() {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Profile" component={Profile} />
      <Tab.Screen name="Settings" component={Settings} />
    </Tab.Navigator>
  );
}

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen
          name="Home"
          component={Home}
          options={{ headerShown: false }}
        />
        <Stack.Screen name="EditPost" component={EditPost} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

在这些示例中，我们使用了直接嵌套在另一个堆栈导航器内部的底部选项卡导航器，但是当中间有其他导航器时，原则同样适用，例如：堆栈导航器位于另一个堆栈导航器内部的选项卡导航器中，或者堆栈导航器内部的抽屉导航器等

如果您不想在任何导航器中显示标题，可以使用 `headerShown: false` 在所有导航器中指定：



6.4 嵌套时的最佳实践

我们建议将嵌套导航器减少到最少。尝试以尽可能少的嵌套来实现您想要的操作。嵌套有很多缺点：

- 它导致了深度嵌套的视图层次结构，这可能会导致低端设备出现内存和性能问题
- 嵌套相同类型的导航器（例如选项卡内的选项卡、抽屉内的抽屉等）可能会导致用户体验混乱
- 由于嵌套过多，在导航到嵌套屏幕、配置深层链接等时，代码变得难以理解。

将嵌套导航器视为实现所需 UI 的一种方式，而不是一种组织代码的方式。如果要为组织创建单独的屏幕组，而不是使用单独的导航器，则可以使用 `Group` 组件。

```html
<Stack.Navigator>
  {isLoggedIn ? (
    // Screens for logged in users
    <Stack.Group>
      <Stack.Screen name="Home" component={Home} />
      <Stack.Screen name="Profile" component={Profile} />
    </Stack.Group>
  ) : (
    // Auth screens
    <Stack.Group screenOptions={{ headerShown: false }}>
      <Stack.Screen name="SignIn" component={SignIn} />
      <Stack.Screen name="SignUp" component={SignUp} />
    </Stack.Group>
  )}
  {/* Common modal screens */}
  <Stack.Group screenOptions={{ presentation: 'modal' }}>
    <Stack.Screen name="Help" component={Help} />
    <Stack.Screen name="Invite" component={Invite} />
  </Stack.Group>
</Stack.Navigator>
```



## 导航器的生命周期

在前面的内容中，我们介绍了React Navigation的一些基本用法，以及路由跳转相关的内容。但是，我们并不知道路由是如何实现页面的跳转和返回的。

如果你对前端Web比较熟悉的话，那么你就会发现当用户从路由A跳转到路由B的时候，A将会被卸载（`componentWillUnmount`方法会被调用）当用户从其他页面返回A页面的时候，A页面又会被重新加载。React 的生命周期方法在React Navigation中仍然有效，不过相比Web的用法不尽相同，且移动端导航更为复杂。

7.1 Example

假设有一个 Stack 导航器，里面有A、B两个页面。从其他页面跳转到A页面(`navigation.navigate('A')`)，它的`componentDidMount`将会被调用。从A页面跳转到B页面(`navigation.push('B')`)，B页面的`compouentDidMount`也会被调用，但是A页面始终在堆栈中保持被加载的状态，因此A页面的`componentWillUnmount`不会被调用。

但是，当我们从B页面返回到A页面的时候，B页面的`compouentWillUnmount`则会被调用，但是A页面的`componentDidMount`不会被调用，因为其没有被卸载，在整个过程中一直保持着被加载的状态。

在其他类型的导航器中也是相同的结果，假设有一个 Tab 导航器，其有两个 Tab，每个 Tab 都是一个 Stack 导航器。

````typescript
function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator>
        <Tab.Screen name="First">
          {() => (
            <SettingsStack.Navigator>
              <SettingsStack.Screen
                name="Settings"
                component={SettingsScreen}
              />
              <SettingsStack.Screen name="Profile" component={ProfileScreen} />
            </SettingsStack.Navigator>
          )}
        </Tab.Screen>
        <Tab.Screen name="Second">
          {() => (
            <HomeStack.Navigator>
              <HomeStack.Screen name="Home" component={HomeScreen} />
              <HomeStack.Screen name="Details" component={DetailsScreen} />
            </HomeStack.Navigator>
          )}
        </Tab.Screen>
      </Tab.Navigator>
    </NavigationContainer>
  );
}
````

首先从 HomeScreen 跳转到 DetailsScreen ，然后切换底部选项卡，从 SettingScreen 跳转到 ProfileScreen。在这一系列操作完成之后，所有的四个页面都已经被加载过了，如果你切换底部选项卡回到HomeStack，你会发现当前呈现的还是 DetailsScreen（HomeStack的导航状态已经被保存了）



7.2 React Navigation 生命周期事件

现在我们知道生命周期函数在 React Navigation 中是如何工作的了，让我们回答我们一开始提出的问题："我们是如何发现用户离开当前页面和返回当前页面的？"

React Navigation 将会发布事件给订阅这些事件的屏幕组件，我们可以监听 `focus` 和 `blur` 事件，来了解当前页面是否被激活

```typescript
function Profile({ navigation }) {
  React.useEffect(() => {
    const unsubscribe = navigation.addListener('focus', () => {
      // Screen was focused
      // Do something
    });

    return unsubscribe;
  }, [navigation]);

  return <ProfileContent />;
}
```

我们可以使用 `useFocusEffect` 钩子监听生命周期来替代手动添加监听器，它类似于 React 的 `useEffect` 钩子，不同的是，`useFocusEffect` 只能监听导航的生命周期

```typescript
import { useFocusEffect } from '@react-navigation/native';

function Profile() {
  useFocusEffect(
    React.useCallback(() => {
      // Do something when the screen is focused

      return () => {
        // Do something when the screen is unfocused
        // Useful for cleanup functions
      };
    }, [])
  );

  return <ProfileContent />;
}
```

如果你想要根据页面是否获得焦点和失去焦点来渲染不同的东西，也可以调用 `useIsFocused` 钩子，`useIsFocused` 会返回一个布尔值，用来指示该页面是否获得了焦点。

总的来说，React 的生命周期方法仍然可用，除此之外，React Navigation又增加了更多的事件方法，开发者可以通过 navigation 属性来订阅他们。并且，开发者可以使用 `useFocusEffect` 或者 `useIsFocused` 钩子。



## 打开一个 Modal 全面屏页面

在 React Native 中，Modal 是一个弹窗组件，它不是导航中的页面，因此其显示与隐藏都有其独特的方式，我们可以使用它展示一些特别的提示信息。

8.1 创建一个Modal 堆栈导航器

首先，我们来看一下如下一段代码：

只在 iOS 中可用

```typescript
function HomeScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text style={{ fontSize: 30 }}>This is the home screen!</Text>
      <Button
        onPress={() => navigation.navigate('MyModal')}
        title="Open Modal"
      />
    </View>
  );
}
 
function DetailsScreen() {
  return (
    <View>
      <Text>Details</Text>
    </View>
  );
}
 
function ModalScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text style={{ fontSize: 30 }}>This is a modal!</Text>
      <Button onPress={() => navigation.goBack()} title="Dismiss" />
    </View>
  );
}
 
const MainStack = createNativeStackNavigator();
const RootStack = createNativeStackNavigator();
 
function MainStackScreen() {
  return (
    <MainStack.Navigator>
      <MainStack.Screen name="Home" component={HomeScreen} />
      <MainStack.Screen name="Details" component={DetailsScreen} />
    </MainStack.Navigator>
  );
}
 
function RootStackScreen() {
  return (
    <RootStack.Navigator  screenOptions={{presentation: 'modal'}}>
      <RootStack.Screen
        name="Main"
        component={MainStackScreen}
        options={{ headerShown: false }}
      />
      <RootStack.Screen name="MyModal" component={ModalScreen} />
    </RootStack.Navigator>
  );
}
```

首先，我们来看一下上面示例中导航器的示意图。

![](https://gitee.com/admvli2016/pictures/raw/master/img/992252573-b0020058b69ae3a3_fix732.png)

在上面的示例中，我们使用 MainStackScreen 作为 RootStackScreen 的屏幕组件，就相当于我们在一个堆栈导航器中嵌入了另一个堆栈导航器。因此，当我们运行项目时，RootStackScreen 会渲染一个堆栈导航器，该导航器有自己的头部，当然我们也可以将这个头部隐藏。

堆栈导航器的 `presentation` 属性值可以是 `card(默认)`或者`modal`。在 iOS 上，modal 的展现方式是从底部滑入，退出的时候则是从上往下滑的方式来关闭它。在 Android 上，modal 是无效的属性，因为全面屏的 modal 与android 的平台自带的行为没有任何区别。

Modal 堆栈导航器在 React Navigation 导航中是非常有用的，如果你想变更堆栈导航器页面的过渡动画，那么就可以使用 `presentation` 属性。当将其设置为 modal 时，所有屏幕转换之间的过渡动画就会变为从底部滑到顶部，而不是原先的从右侧滑入。但是，需要注意的是，React Navigation 的 modal 会对整个堆栈导航器起效，所以为了在其他屏幕上使用从右侧滑入的效果，我们可以再另外添加一个导航器，这个导航器使用默认的配置就行。



## 名称解释

9.1 Navigator

Navigator 是一个 React 组件，它决定应用以哪种方式展现已定义的页面。NavigationContainer 则是一个管理导航树并包含导航状态的组件，该组件是所有导航器组件的父容器，包装了所有导航器的结构。通常，我们需要将NavigationContainer 组件放在应用程序的根目录下，作为启动的根文件（App.js）。

```typescript
function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator> 
        <Stack.Screen name="Home" component={HomeScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
    // <---- This is a Navigator
}
```



9.2 Router

Router 即是路由器，它是一组函数，它决定如何在导航器中处理用户的操作和状态的更改，通常，开发者不需要直接参与路由器交互，除非您需要编写自定义导航器。



9.3 Screen component

Screen component 即屏幕组件，是我们在路由配置中需要跳转的组件。

```typescript
const Stack = createStackNavigator();

const StackNavigator = (
  <Stack.Navigator>
    <Stack.Screen
      name="Home"
      component={HomeScreen} />
    <Stack.Screen
      name="Details"
      component={DetailsScreen} />
  </Stack.Navigator>
);
```

需要注意的是，只有当屏幕被 React Navigation 呈现为一个路由时才能实现跳转功能。例如，如果我们渲染DetailsScreen 作为主屏幕的子元素，那么 DetailsScreen 将不会提供导航道具，当你在主屏幕上按下 “Go to Details…”  按钮时应用程序会抛出 "undefined is not an object " 的错误。

```typescript
function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
      <Button
        title="Go to Details"
        onPress={() => navigation.navigate('Details')}
      />
      <DetailsScreen />
    </View>
  );
}
```



9.4 Navigation Prop

通过使用 Navigation 提供的属性，我们可以在两个页面之间传递数据，常用的属性如下

- dispatch： 向路由发送一个 action
- navigate，goBack：打开或者返回页面



9.5 Navigation State

导航器的状态如下所示

```js
{
  key: 'StackRouterRoot',
  index: 1,
  routes: [
    { key: 'A', name: 'Home' },
    { key: 'B', name: 'Profile' },
  ]
}
```



9.6 Route Prop

Route 提供的属性，可以被传递给所有的屏幕。包含有关当前 route 的信息，例如 params，key 和 name



9.7 Route

每个路由都是一个对象，其中包含标识它的键和指定路由类型的“名称”，并且还可以包含任意类型的参数。

```js
{
  key: 'B',
  name: 'Profile',
  params: { id: '123' }
}
```



9.8 Header

也称为导航标题、导航栏、应用栏，可能还有许多其他东西。这是屏幕顶部的矩形，包含后退按钮和屏幕标题。

整个矩形在 React Navigation 中通常称为标题。



参考博文：

[React Navigation 5.x详解 - SegmentFault 思否](https://segmentfault.com/a/1190000038346668)

[(30条消息) react-navigation导航组件使用详解_栀夏暖阳的博客-CSDN博客_react 导航](https://blog.csdn.net/kisty_yao/article/details/105363598)

[Getting started | React Navigation](https://reactnavigation.org/docs/getting-started)