# Tools
```
brew install node
```

# Init project
```bash
touch .env .gitignore README.md
mkdir src
mkdir src/js src/views src/views/components src/images src/scss
touch src/views/index.html
touch src/js/main.js
touch webpack.config.js
touch .eslintrc
touch .stylelintrc
touch posthtml.json
echo "node_modules\n.env" > .gitignore
npm init -y
npm i -D dotenv onchange node-sass autoprefixer postcss-cli css-loader node-sass postcss-loader sass-loader style-loader stylelint browser-sync npm-run-all imagemin-cli webpack webpack-cli babel-loader @babel/preset-env eslint eslint-webpack-plugin posthtml posthtml-cli posthtml-modules htmlnano 
npm i bootstrap@4.3.1 jquery
```


# Resources
Build static website without no framework:
https://wweb.dev/blog/how-to-create-static-website-npm-scripts
Adding bootstrap an jquery:
https://stevenwestmoreland.com/2018/01/how-to-include-bootstrap-in-your-project-with-webpack.html
https://getbootstrap.com/docs/4.0/getting-started/webpack/