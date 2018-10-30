# Chap11 Get Cookie From Server

### examples

{% embed url="https://github.com/zeit/next.js/blob/5ff7c0742c25394ff8384ee31915bddaac46a4f2/examples/with-apollo-auth/lib/withApollo.js" %}

{% embed url="https://github.com/zeit/next.js/issues/2364" %}



{% embed url="https://github.com/luisrudge/next.js-auth0/blob/master/utils/auth.js" %}



{% code-tabs %}
{% code-tabs-item title="pages/\_app.js" %}
```jsx
const user = process.browser ? Cookie.getJSON('user') : serverCookie.getJSON(ctx.req, 'user')
const expiresAt = process.browser ? Cookie.get('expiresAt') : serverCookie.get(ctx.req, 'expiresAt')
```
{% endcode-tabs-item %}
{% endcode-tabs %}

When the process is "browser", `Cookie.getJSON` works.

But... when the process is "server", we need to get Cookie from server request.



{% code-tabs %}
{% code-tabs-item title="utils/serverCookie.js" %}
```javascript
function get (req, cookieName) {
  if (!req.headers.cookie) return undefined // ex: browser secret mode
  const cookies = req.headers.cookie
  // console.log(cookies) => user={%22id%22:1%2C%22image%22:%22https:/wq91tt4Js6s/photo.jpg%22}; accessToken=CaLaMcTKmd; expiresAt=1536124075280
  const cookie = cookies.split(';').find(c => c.trim().startsWith(`${cookieName}=`))

  if (!cookie) return undefined // if there is no cookie matches cookieName

  const data = cookie.split('=')[1] // console.log(cookie) => expiresAt=1536124075280
  /* decode cookie (special char is encoded by js-cookie) */
  return decodeURIComponent(data)
}

function getJSON (req, cookieName) {
  if (!req.headers.cookie) return undefined // ex: browser secret mode
  const cookies = req.headers.cookie
  const cookie = cookies.split(';').find(c => c.trim().startsWith(`${cookieName}=`))

  if (!cookie) return {} // if there is no cookie matches cookieName

  const data = cookie.split('=')[1]

  /* decode cookie (special char is encoded by js-cookie). */
  /* Then, parse to Json from string */
  // https://github.com/js-cookie/js-cookie#encoding
  // https://stackoverflow.com/questions/29797946/handling-bad-json-parse-in-node-safely
  let jsonData
  try {
    jsonData = JSON.parse(decodeURIComponent(data))
  } catch (e) {
    jsonData = {}
  }
  return jsonData
}

module.exports = {
  get,
  getJSON
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}





### Check !!

It worked!



{% embed url="https://stackoverflow.com/questions/29797946/handling-bad-json-parse-in-node-safely" %}

