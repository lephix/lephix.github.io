# Note for React

## links

[React official web site](https://reactjs.org)

[create react app](https://github.com/facebook/create-react-app)


## Startup

```bash
npx create-react-app react-test
cd react-test
npm start
```

## React

### Demo with JSX

JSX is a mixture of HTML and javascript. Content with in `<>` will be parsed as HTML, and in `{}` will be parsed as Javascript.

```jsx
# index.js
import React from 'react';
import ReactDOM from 'react-dom';

const word = 'React';
ReactDOM.render(<h1> Hello { word } </h1>, document.getElementById('root'));

```

### Component

```jsx
# App.js
import React from 'react';

class App extends React.Component {
  render() {
    return (
      <div>
        <h1>This a Component named App</h1>
        {
          this.props.people.map((element, index) => {
            return <li key={index}>{element}</li>
          })
        }
      </div>
    )
  }
}

export default App
```

```jsx
# index.js
import React from 'react';
import ReactDOM from 'react-dom';

const word = 'React';
ReactDOM.render(<App people={ ['Alpha', 'Beta'] }></App>, document.getElementById('root'));
```

