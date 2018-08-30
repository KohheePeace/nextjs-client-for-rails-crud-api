# Chap14 posts\#new page

Todo 

* [ ] make plain form with styles
* [ ] customAxios
* [ ] isAuthenticated compoentns
* [ ] addMessage Notifier



### make plain form with styles

{% code-tabs %}
{% code-tabs-item title="pages/posts/new.js" %}
```jsx
import Typography from '@material-ui/core/Typography'
import TextField from '@material-ui/core/TextField'
import Button from '@material-ui/core/Button'
import Paper from '@material-ui/core/Paper'
import { withStyles } from '@material-ui/core/styles'
import Router from 'next/router'
import axios from 'axios'
import CircularProgress from '@material-ui/core/CircularProgress'

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
      url: 'http://api.localhost:3001/posts',
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



There are some code which is commented out. These will be implemented in the future chapter.

