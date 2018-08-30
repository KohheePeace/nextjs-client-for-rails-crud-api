# Chap17 Notifier

In this chapter we will implement the Notification message when successfully created Post.



{% code-tabs %}
{% code-tabs-item title="components/common/Notifier.js" %}
```jsx
import React from 'react'
import Snackbar from '@material-ui/core/Snackbar'
import IconButton from '@material-ui/core/IconButton'
import CloseIcon from '@material-ui/icons/Close'

export let addMessage

export default class Notifier extends React.Component {
  constructor (props) {
    super(props)
    this.state = {
      open: false,
      message: ''
    }
  }

  componentDidMount () {
    addMessage = this.addMessage
  }

  handleClose = (event, reason) => {
    if (reason === 'clickaway') {
      return
    }

    this.setState({
      open: false
    })
  };

  addMessage = (message) => {
    this.setState({
      open: true,
      message: message
    })
  }

  render () {
    const { message, open } = this.state
    return (
      <Snackbar
        anchorOrigin={{ vertical: 'top', horizontal: 'right' }}
        message={message}
        autoHideDuration={3000}
        onClose={this.handleClose}
        open={open}
        ContentProps={{
          'aria-describedby': 'message-id'
        }}
        action={[
          <IconButton
            key='close'
            aria-label='Close'
            color='inherit'
            onClick={this.handleClose}
          >
            <CloseIcon />
          </IconButton>
        ]}
      />
    )
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Almost of code is same with material-ui example [https://material-ui.com/demos/snackbars/\#simple](https://material-ui.com/demos/snackbars/#simple)



Import it in `pages/_app.js` and Render Notifier Component.

{% code-tabs %}
{% code-tabs-item title="pages/\_app.js" %}
```jsx
import Notifier from '../components/common/Notifier'

...
<Notifier />
<Header isAuthenticated={isAuthenticated} user={user} />
```
{% endcode-tabs-item %}
{% endcode-tabs %}



### \*Note!

Export state change function is "special case".

The special case is like... **Cutstom Alert component**

_Only one instance of &lt;customAlert /&gt; mounted at a time._

The way to achieve this ...

1.use 'ref' to controll DOM

2.attach state changing function to 'window'

[3.my](https://3.my/) answer=&gt; export state changing function and import from other file.



{% embed data="{\"url\":\"https://medium.com/@veelenga/displaying-rails-flash-messages-with-react-5f82982f241c\",\"type\":\"link\",\"title\":\"Displaying Rails flash messages with React\",\"description\":\"Rails has a nice and useful mechanism to communicate to user via flash messages. By default these messages come from backend and are…\",\"icon\":{\"type\":\"icon\",\"url\":\"https://cdn-images-1.medium.com/fit/c/304/304/1\*8I-HPL0bfoIzGied-dzOvA.png\",\"width\":152,\"height\":152,\"aspectRatio\":1},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://cdn-images-1.medium.com/max/2000/1\*tR7DdtgNq\_hEABhr5BxctA.gif\",\"width\":1176,\"height\":542,\"aspectRatio\":0.4608843537414966}}" %}



{% embed data="{\"url\":\"https://medium.freecodecamp.org/how-to-show-informational-messages-using-material-ui-in-a-react-web-app-5b108178608\",\"type\":\"link\",\"title\":\"How to show in-app messages using Material-UI in a React web app\",\"description\":\"In some situations, your web app needs to show an informational message to tell users whether an event was successful or not. For example…\",\"icon\":{\"type\":\"icon\",\"url\":\"https://cdn-images-1.medium.com/fit/c/304/304/1\*MotlWcSa2n6FrOx3ul89kw.png\",\"width\":152,\"height\":152,\"aspectRatio\":1},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://cdn-images-1.medium.com/max/1364/1\*awO-09f5MGtVvnkbVLoBDA.png\",\"width\":682,\"height\":363,\"aspectRatio\":0.532258064516129}}" %}

{% embed data="{\"url\":\"https://itnext.io/understanding-the-react-context-api-through-building-a-shared-snackbar-for-in-app-notifications-6c199446b80c\",\"type\":\"link\",\"title\":\"Learn the React Context API with a Practical Example You Can Bring to Your Apps\",\"description\":\"Building a Shared Material UI Snackbar for In-App Notifications\",\"icon\":{\"type\":\"icon\",\"url\":\"https://cdn-images-1.medium.com/fit/c/304/304/1\*yAqDFIFA5F\_NXalOJKz4TA.png\",\"width\":152,\"height\":152,\"aspectRatio\":1},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://cdn-images-1.medium.com/max/1024/1\*tDxVH\_WUqvu7fiNorh\_EfQ.png\",\"width\":512,\"height\":444,\"aspectRatio\":0.8671875}}" %}



{% embed data="{\"url\":\"https://next.plnkr.co/edit/37nutSDTWp8GGv2r\",\"type\":\"link\",\"title\":\"Change state from anywhere without Redux\",\"description\":\"Created on https://plnkr.co: Helping you build the web.\",\"icon\":{\"type\":\"icon\",\"url\":\"https://next.plnkr.co/favicon.ico\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://shot.plnkr.co/37nutSDTWp8GGv2r.png\",\"width\":400,\"height\":300,\"aspectRatio\":0.75}}" %}

{% embed data="{\"url\":\"https://next.plnkr.co/edit/EpLm1Bq3ASiWECoE\",\"type\":\"link\",\"title\":\"Context State Change\",\"description\":\"Created on https://plnkr.co: Helping you build the web.\",\"icon\":{\"type\":\"icon\",\"url\":\"https://next.plnkr.co/favicon.ico\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://shot.plnkr.co/EpLm1Bq3ASiWECoE.png\",\"width\":400,\"height\":300,\"aspectRatio\":0.75}}" %}

### Use addMessage

When you want to show Notification message, just call `addMessage` function.

{% code-tabs %}
{% code-tabs-item title="pages/posts/new.js" %}
```jsx
...
import { addMessage } from '../../components/common/Notifier'
...

addMessage('successfully create Post!')
...
addMessage('Something wrong happened!')
```
{% endcode-tabs-item %}
{% endcode-tabs %}

and

pages/login.js

```jsx
import { addMessage } from '../components/common/Notifier'

setToken(user, accessToken, expiresAt)
addMessage('Successfully Login!')
Router.push('/')
```

### Check it!

It worked! okay!

