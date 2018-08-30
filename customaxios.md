# Chap16 NotAuthenticated.js

In this chapter, we will make NotAuthenticated.js component.

### Check Current behavior

You can see `posts/new` page without login.

![](.gitbook/assets/sukurnshotto-2018-08-25-43655.png)



To avoid this...

Return `NotAuthenticated` component when user is not authenticated. 

```jsx
render () {
    const { classes, isAuthenticated } = this.props
    const { title, content, isLoading } = this.state
    if (!isAuthenticated) {
      return <NotAuthenticated />
    }
    ...
  }
```

Please Uncomment these code

{% code-tabs %}
{% code-tabs-item title="pages/posts/new.js" %}
```jsx
if (!isAuthenticated) {
  return <NotAuthenticated />
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



Let's make the component!

{% code-tabs %}
{% code-tabs-item title="components/common/NotAuthenticated.js" %}
```jsx
import Typography from '@material-ui/core/Typography'
import Link from 'next/link'

export default () => (
  <div className='root'>
    <Typography variant='display1' gutterBottom>
      {"You're not authenticated yet."}
    </Typography>
    <div>
      Please
      <Link href='/login'>
        <a>{`  `}Log In</a>
      </Link>
    </div>
  </div>
)
```
{% endcode-tabs-item %}
{% endcode-tabs %}



And Import it

{% code-tabs %}
{% code-tabs-item title="pages/posts/new.js" %}
```jsx
import NotAuthenticated from '../../components/common/NotAuthenticated'
```
{% endcode-tabs-item %}
{% endcode-tabs %}



### Check

It worked!

![](.gitbook/assets/sukurnshotto-2018-08-25-185757.png)







