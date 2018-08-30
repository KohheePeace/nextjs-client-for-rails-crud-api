# Chap1 Nextjs Setup

{% embed data="{\"url\":\"https://nextjs.org/learn/basics/getting-started/setup\",\"type\":\"link\",\"title\":\"Getting Started - Learn Next.js\"}" %}



```text
mkdir hello-next-crud
cd hello-next-crud
yarn init -y
yarn add react react-dom next
mkdir pages
```



```text
touch .gitignore
```



```text
node_modules
.next
yarn-error.log
```



[https://github.com/luisrudge/next.js-boilerplate/blob/master/.gitignore](https://github.com/luisrudge/next.js-boilerplate/blob/master/.gitignore)

[https://stackoverflow.com/questions/42592168/should-i-add-yarn-error-log-to-my-gitignore-file](https://stackoverflow.com/questions/42592168/should-i-add-yarn-error-log-to-my-gitignore-file)



```text
git init
```



Add NPM script.

```text
{
  "scripts": {
    "dev": "next"
  }
}
```



{% code-tabs %}
{% code-tabs-item title="package.json" %}
```javascript
{
  "name": "hello-next-crud",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "scripts": {
    "dev": "next"
  },
  "dependencies": {
    "next": "^6.1.1",
    "react": "^16.4.2",
    "react-dom": "^16.4.2"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



```text
yarn run dev
```



Visit

[http://localhost:3000/](http://localhost:3000/)



![](.gitbook/assets/sukurnshotto-2018-08-16-90233.png)



Create a file named `pages/index.js` and add the following content:  


{% code-tabs %}
{% code-tabs-item title="pages/index.js" %}
```javascript
const Index = () => (
  <div>
    <p>Hello Next.js</p>
  </div>
)

export default Index
```
{% endcode-tabs-item %}
{% endcode-tabs %}



Now if you visit [http://localhost:3000](http://localhost:3000/) again, you'll see a page with "Hello Next.js"  


![](.gitbook/assets/sukurnshotto-2018-08-16-90412.png)







