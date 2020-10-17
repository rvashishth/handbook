## Fundamentals

- [HTML Server Sent Event API](https://www.w3schools.com/html/html5_serversentevents.asp)

## React Ecosystem

- ReactJS
- [Redux](https://redux.js.org/)
- [Redux Thunk](https://github.com/reduxjs/redux-thunk)
- [React Redux](https://react-redux.js.org/)
  - Redux selectors - Redux Store Destructuring i.e. converting a map to list
- [React Router](https://reacttraining.com/react-router/)
- Redux OIDC
- [Redux Ducks Pattern](https://github.com/erikras/ducks-modular-redux)
- [Redux Re-Ducks Pattern](https://github.com/alexnm/re-ducks)
- [React Final Forms](https://github.com/final-form/react-final-form)
- [React Table](https://github.com/tannerlinsley/react-table)
- TypeScript
- Webpack
- [Material UI](https://material-ui.com/)
- [Styled Components](https://styled-components.com/)
- Axios
- Babel
- [Jest](https://jestjs.io/)
- [Enzyme](https://airbnb.io/enzyme/)
- [React S3 Uploader](https://github.com/odysseyscience/react-s3-uploader)
- [URI JS](https://github.com/medialize/URI.js)
- [React Select](https://react-select.com)
- [Reselect](https://github.com/reduxjs/reselect)
- [React Toastify](https://github.com/fkhadra/react-toastify)
- [TestCafe - acceptance test](https://devexpress.github.io/testcafe/)
- ESLint
- Yarn
- [Detect Browser](https://github.com/DamonOehlman/detect-browser)
- [Immutable.js Immutable collections for JavaScript](https://immutable-js.github.io/immutable-js/)
- [Immer - work with immutable state](https://immerjs.github.io/) - in react-redux selectors
- [https://storybook.js.org/](https://storybook.js.org/)
- [Simplur - string pluralization](https://github.com/broofa/BroofaJS/tree/master/simplur)

## Blogs/Sites

- https://osawards.com/javascript/
- https://osawards.com/react/

## Utilities

- [http://latentflip.com/loupe/](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D) - demo callback eventloop

### fake/mock/dummy data api

- https://unsplash.com/documentation - photos like user profile,
- http://marak.github.io/faker.js/ - generate fake data
- https://jsonplaceholder.typicode.com/ - fake data api

## Good Read

- [ReactJS Tutorial for Beginners - codeevolution by Vishwash - youtube course](https://www.youtube.com/playlist?list=PLC3y8-rFHvwgg3vaYJgHGnModB54rxOk3)
- [React lifecycle methods](https://engineering.opsgenie.com/i-wish-i-knew-these-before-diving-into-react-301e0ee2e488)
- [How JSX prevent injection attacks /cross site scripting attacks(XSS)](https://reactjs.org/docs/introducing-jsx.html#jsx-prevents-injection-attacks)
- [Understanding JS Bing](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/)

## [VS Code extensions](../../Basic/vscode.md)

## FAQ

- **Why do we need index.js**</br>
  Import, modules. Webpack also inserts index.js if not already present
- **we export container from index.js but import the actual component, why**</br>

## React/JS Best Practices

- Keep sorted data in redux
- Use padding instead of margin, wherever applicable
- Do care about truthy/falsy checks
- Use optional chaining
- [react performance](https://alligator.io/react/keep-react-fast/)
  - use `memo` and `pureComponent`
  - Avoid anonymous functions
  - Avoid object literals
  - use React.lazy and React.suspense
  - Instead of unmount component to hide, use css props to make it disappear

## TODO

### JS & TS

- [ ] What is [automatic semicolon insertion](https://stackoverflow.com/questions/2846283/what-are-the-rules-for-javascripts-automatic-semicolon-insertion-asi)
- [ ] Write an article on JS class vs TS class

### React

- [ ] Why we should/should not have const function in class
- [ ] Why we should not create const array functions in class components
