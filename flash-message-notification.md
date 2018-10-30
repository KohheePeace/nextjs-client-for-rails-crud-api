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



{% embed url="https://medium.com/@veelenga/displaying-rails-flash-messages-with-react-5f82982f241c" %}



{% embed url="https://medium.freecodecamp.org/how-to-show-informational-messages-using-material-ui-in-a-react-web-app-5b108178608" %}

{% embed url="https://itnext.io/understanding-the-react-context-api-through-building-a-shared-snackbar-for-in-app-notifications-6c199446b80c" %}



{% embed url="https://next.plnkr.co/edit/37nutSDTWp8GGv2r" %}

{% embed url="https://next.plnkr.co/edit/EpLm1Bq3ASiWECoE" %}

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

