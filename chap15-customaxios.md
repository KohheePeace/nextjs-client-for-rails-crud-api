# Chap15 customAxios

Currently, `posts/new.js`is like this.

{% code-tabs %}
{% code-tabs-item title="pages/posts/new.js" %}
```jsx
import Typography from '@material-ui/core/Typography'
import TextField from '@material-ui/core/TextField'
import Button from '@material-ui/core/Button'
import Paper from '@material-ui/core/Paper'
import { withStyles } from '@material-ui/core/styles'
import CircularProgress from '@material-ui/core/CircularProgress'
import Router from 'next/router'
import axios from 'axios'
import Cookie from 'js-cookie'

const styles = {
  root: {
    textAlign: 'center',
    paddingTop: '3rem'
  },
  paper: {
    padding: '20px 40px',
    maxWidth: 480,
    margin: '0 auto'
  }
}

class PostsNew extends React.Component {
  state = {
    title: '',
    content: '',
    isLoading: false
  }

  handleInputChange = name => event => {
    this.setState({
      [name]: event.target.value
    })
  };

  handleSubmit = (event) => {
    event.preventDefault()
    this.setState({ isLoading: true })

    const { title, content } = this.state

    axios({
      method: 'post',
      url: 'http://api.localhost:3000/posts',
      data: {post: { title, content }},
      headers: {'Authorization': `Bearer ${Cookie.getJSON('accessToken')}`}
    })
      .then((res) => {
        this.setState({
          isLoading: false
        })
        const { data } = res
        const { id, slug } = data
        // addMessage('successfully create Post!')

        /* Router.push(url, as) */
        // https://github.com/zeit/next.js/issues/1222#issuecomment-281191907
        Router.push(`/posts/show?id=${id}`, `/posts/${slug}`)
      })
      .catch((error) => {
        this.setState({
          isLoading: false
        })
        // addMessage('Something wrong happened!')
      })
  }

  render () {
    const { classes, isAuthenticated } = this.props
    const { title, content, isLoading } = this.state
    // if (!isAuthenticated) {
    //   return <NotAuthenticated />
    // }
    return (
      <div className={classes.root}>
        <Typography variant='display1' gutterBottom>Add New Post</Typography>
        <Paper className={classes.paper}>
          <form className='form' onSubmit={this.handleSubmit}>
            <TextField
              id='title'
              label='Title'
              value={title}
              onChange={this.handleInputChange('title')}
              margin='normal'
              fullWidth
            />
            <TextField
              id='content'
              label='Content'
              value={content}
              onChange={this.handleInputChange('content')}
              margin='normal'
              fullWidth
              multiline
              rows='7'
            />
            <Button
              variant='raised'
              type='submit'
              color='secondary'
              fullWidth
              disabled={isLoading}
              style={{marginTop: 20}}
            >
              {isLoading ? <CircularProgress size={24} /> : 'Save'}
            </Button>
          </form>
        </Paper>
      </div>
    )
  }
}
export default withStyles(styles)(PostsNew)
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### In this chapter

In this chapter, we will focus axios.

```jsx
axios({
      method: 'post',
      url: 'http://api.localhost:3001/posts',
      data: {post: { title, content }},
      headers: {'Authorization': `Bearer ${Cookie.getJSON('accessToken')}`}
    })
      .then((res) => {
        ...
      })
      .catch((error) => {
        ...
      })
  }
```

In this http request...

* Only authenticated user can add new post.

-&gt; So, we need to send `accessToken` 

* to create a new post we need to send data `title` and `content`

-&gt; This data is different in each case of your backend. Don't need to  care too much.



What is needed to refactor axios is that...

It is troublesome to write some same code of this part.

```text
axios({
    method: 'post',
    url: 'http://api.localhost:3001/posts',
    data: {post: { title, content }},
    headers: {'Authorization': `Bearer ${Cookie.getJSON('accessToken')}`}
})
```

To avoid writing same part of code, lets make custom axios instance.

{% code-tabs %}
{% code-tabs-item title="utils/customAxios.js" %}
```javascript
import axios from 'axios'
import Cookie from 'js-cookie'

const dev = process.env.NODE_ENV !== 'production'
const baseURL = dev ? 'http://api.localhost:3001/' : 'https://api.production.com/'

const instance = axios.create({
  baseURL: baseURL,
  timeout: 30000
})

// https://github.com/axios/axios/issues/1383
instance.interceptors.request.use(function (config) {
  config.headers.Authorization = `Bearer ${Cookie.getJSON('accessToken')}`
  return config
}, function (error) {
  // Do something with request error
  return Promise.reject(error)
})

export default instance
```
{% endcode-tabs-item %}
{% endcode-tabs %}

[Reference here](https://github.com/axios/axios/issues/1383).

You may come up with like the below code...

```javascript
import axios from 'axios'
import Cookie from 'js-cookie'

const dev = process.env.NODE_ENV !== 'production'
const baseURL = dev ? 'http://api.localhost:3001/' : 'https://api.production.com/'

const instance = axios.create({
  baseURL: baseURL,
  timeout: 30000,
  headers: {'Authorization': `Bearer ${Cookie.getJSON('accessToken')}`}
})

export default instance
```

**But... this doesn't work.**

> Axios instance is not updated even if Cookie is updated.



### Refactor `pages/posts/new.js` 

Import `customAxios`

```text
import axios from '../../utils/customAxios'
```

Delete `axios` and `js-cookie`

Refactor axios

```text
axios({
    method: 'post',
    url: '/posts',
    data: {post: { title, content }},
})
```



So...

{% code-tabs %}
{% code-tabs-item title="pages/posts/new.js" %}
```jsx
import Typography from '@material-ui/core/Typography'
import TextField from '@material-ui/core/TextField'
import Button from '@material-ui/core/Button'
import Paper from '@material-ui/core/Paper'
import { withStyles } from '@material-ui/core/styles'
import CircularProgress from '@material-ui/core/CircularProgress'
import Router from 'next/router'
import axios from '../../utils/customAxios'

const styles = {
  root: {
    textAlign: 'center',
    paddingTop: '3rem'
  },
  paper: {
    padding: '20px 40px',
    maxWidth: 480,
    margin: '0 auto'
  }
}

class PostsNew extends React.Component {
  state = {
    title: '',
    content: '',
    isLoading: false
  }

  handleInputChange = name => event => {
    this.setState({
      [name]: event.target.value
    })
  };

  handleSubmit = (event) => {
    event.preventDefault()
    this.setState({ isLoading: true })

    const { title, content } = this.state

    axios({
      method: 'post',
      url: '/posts',
      data: {post: { title, content }},
    })
      .then((res) => {
        this.setState({
          isLoading: false
        })
        const { data } = res
        const { id, slug } = data
        // addMessage('successfully create Post!')

        /* Router.push(url, as) */
        // https://github.com/zeit/next.js/issues/1222#issuecomment-281191907
        Router.push(`/posts/show?id=${id}`, `/posts/${slug}`)
      })
      .catch((error) => {
        this.setState({
          isLoading: false
        })
        // addMessage('Something wrong happened!')
      })
  }

  render () {
    const { classes, isAuthenticated } = this.props
    const { title, content, isLoading } = this.state
    // if (!isAuthenticated) {
    //   return <NotAuthenticated />
    // }
    return (
      <div className={classes.root}>
        <Typography variant='display1' gutterBottom>Add New Post</Typography>
        <Paper className={classes.paper}>
          <form className='form' onSubmit={this.handleSubmit}>
            <TextField
              id='title'
              label='Title'
              value={title}
              onChange={this.handleInputChange('title')}
              margin='normal'
              fullWidth
            />
            <TextField
              id='content'
              label='Content'
              value={content}
              onChange={this.handleInputChange('content')}
              margin='normal'
              fullWidth
              multiline
              rows='7'
            />
            <Button
              variant='raised'
              type='submit'
              color='secondary'
              fullWidth
              disabled={isLoading}
              style={{marginTop: 20}}
            >
              {isLoading ? <CircularProgress size={24} /> : 'Save'}
            </Button>
          </form>
        </Paper>
      </div>
    )
  }
}
export default withStyles(styles)(PostsNew)
```
{% endcode-tabs-item %}
{% endcode-tabs %}



### Refactor axios everwhere else

{% code-tabs %}
{% code-tabs-item title="pages/index.js" %}
```jsx
import axios from '../utils/customAxios'
...
const res = await axios.get('/posts')
```
{% endcode-tabs-item %}
{% endcode-tabs %}



{% code-tabs %}
{% code-tabs-item title="pages/login.js" %}
```jsx
import axios from '../utils/customAxios'
...
axios({
    method: 'post',
    url: '/auth/google_oauth2/callback',
    params: {code: response.code},
    headers: {'X-Requested-With': 'XMLHttpRequest'}
})
```
{% endcode-tabs-item %}
{% endcode-tabs %}



{% code-tabs %}
{% code-tabs-item title="pages/posts/show.js" %}
```jsx
import axios from '../../utils/customAxios'
...
const res = await axios.get(`/posts/${id}`)
```
{% endcode-tabs-item %}
{% endcode-tabs %}



### Finish!

