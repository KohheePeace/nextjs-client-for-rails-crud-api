# Logout

1. Delete token from server
2. delete token from client cookie
3. if user request \#logout...

やること：サーバーから、tokenを消して、動くかかくにん

{% code-tabs %}
{% code-tabs-item title="utils/auth.js" %}
```javascript
export const logout = async () => {
  if (!process.browser) {
    return
  }

  // delete sever side access token
  await axios.delete('/logout').catch((error) => console.log(error))

  Cookie.remove('accessToken')
  Cookie.remove('user')
  Cookie.remove('expiresAt')

  Router.push('/')
  addMessage('successfully logout!')
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}





