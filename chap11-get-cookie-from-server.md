# Chap11 Get Cookie From Server

### examples

{% embed data="{\"url\":\"https://github.com/zeit/next.js/blob/5ff7c0742c25394ff8384ee31915bddaac46a4f2/examples/with-apollo-auth/lib/withApollo.js\",\"type\":\"link\",\"title\":\"zeit/next.js\",\"description\":\"next.js - Next.js is a lightweight framework for static and server‑rendered applications.\",\"icon\":{\"type\":\"icon\",\"url\":\"https://github.com/fluidicon.png\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://avatars3.githubusercontent.com/u/14985020?s=400&v=4\",\"width\":400,\"height\":400,\"aspectRatio\":1}}" %}

{% embed data="{\"url\":\"https://github.com/zeit/next.js/issues/2364\",\"type\":\"link\",\"title\":\"How to use getInitialProps to get cookies? · Issue \#2364 · zeit/next.js\",\"description\":\"I&\#39;m facing an issue with next.js I cannot get my cookies when i make a request from async static getInitialProps. i get undefined However when i am making it in componentWillMount there is no p...\",\"icon\":{\"type\":\"icon\",\"url\":\"https://github.com/fluidicon.png\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://avatars3.githubusercontent.com/u/10157316?s=400&v=4\",\"width\":420,\"height\":420,\"aspectRatio\":1}}" %}



{% embed data="{\"url\":\"https://github.com/luisrudge/next.js-auth0/blob/master/utils/auth.js\",\"type\":\"link\",\"title\":\"luisrudge/next.js-auth0\",\"description\":\"next.js-auth0 - a simple example that shows how to use next.js with auth0\",\"icon\":{\"type\":\"icon\",\"url\":\"https://github.com/fluidicon.png\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://avatars2.githubusercontent.com/u/941075?s=400&v=4\",\"width\":400,\"height\":400,\"aspectRatio\":1}}" %}



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



{% embed data="{\"url\":\"https://stackoverflow.com/questions/29797946/handling-bad-json-parse-in-node-safely\",\"type\":\"link\",\"title\":\"Handling bad JSON.parse\(\) in node safely\",\"description\":\"Using node/express -  I want to get some JSON out of request headers, but I want to do it safely. If for some reason it\'s not valid JSON, it\'s fine, it can just return false or whatever and it will...\",\"icon\":{\"type\":\"icon\",\"url\":\"https://cdn.sstatic.net/Sites/stackoverflow/img/apple-touch-icon.png?v=c78bd457575a\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://cdn.sstatic.net/Sites/stackoverflow/img/apple-touch-icon@2.png?v=73d79a89bded\",\"width\":316,\"height\":316,\"aspectRatio\":1}}" %}

