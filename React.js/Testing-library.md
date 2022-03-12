# React Testing Library

## Exemple de test

```Javascript
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});
```

On peut remplacer test(...) par it(...)

## Methodes de l'objet screen

L'objet screen possède plusieurs types de methodes avec des propriétés différentes les voici :

|          |  getBy | findBy | queryBy | getAllBy | findAllBy | queryAllBy |
|:--------:|:------:|:------:|:-------:|:--------:|:---------:|:----------:|
| No Match |  error |  error |   null  |   error  |   error   |    array   |
|  1 Match | return | return |  return |   array  |   array   |    array   |
| 1+ Match |  error |  error |  error  |   array  |   array   |    array   |
|   Await  |   no   |   yes  |    no   |    no    |    yes    |     no     |

## Création d'un test unitaire

Un test unitaire test le fonctionnement d'un composant de manière isolée.

Pour cet exemple, on va tester un composant Header :

Header.js :
```Javascript
const Header = ({ title }) => {
    return <h1 className="header">{title}</h1>
}

export default Header
});
```

Dans le dossier Header (s'il existe) on va créer un dossier `__test__`, et dans ce dossier, un fichier `header.test.js`.

Header.test.js :
```Javascript
import { render, screen } from '@testing-library/react';
import Header from '../Header';

it('shoult render same text passed into title prop', () => {
    render(<Header title="My Header"/>);
    const headingElement = screen.getByText(/my header/i);
    expect(headingElement).toBeInTheDocument();
});

it('shoult render same text passed into title prop', () => {
    render(<Header title="My Header"/>);
    const headingElement = screen.getByRole("heading", {name: "My Header"});
    expect(headingElement).toBeInTheDocument();
});

it('shoult render same text passed into title prop', async () => {
    render(<Header title="My Header"/>);
    const headingElement = await screen.findByRole("heading", {name: "My Header"});
    expect(headingElement).toBeInTheDocument();
});

it('shoult render same text passed into title prop', () => {
    render(<Header title="My Header"/>);
    const headingElement = screen.queryByText(/dogs/i);
    expect(headingElement).not.toBeInTheDocument();
});

it('shoult render same text passed into title prop', () => {
    render(<Header title="My Header"/>);
    const headingElements = screen.getAllByRole("heading");
    expect(headingElements.length).toBe(1);
});
```

## Cas spécifiques

Pour ce composant :

```Javascript
import React from 'react'
import "./TodoFooter.css"
import { Link } from "react-router-dom"

function TodoFooter({
    numberOfIncompleteTasks
}) {
    return (
        <div className="todo-footer">
            <p>{numberOfIncompleteTasks} {numberOfIncompleteTasks === 1 ? "task" : "tasks"} left</p>
            <Link to="/followers">Followers</Link>
        </div>
    )
}

export default TodoFooter
```

Un test unitaire comme ci dessus échourait car le composant `Link` n'est pas wrappé dans un composant `BrowserRouter`.

On procède donc comme ceci :

```Javascript
import { render, screen } from '@testing-library/react';
import TodoFooter from "../TodoFooter"
import { BrowserRouter } from "react-router-dom"

const MockTodoFooter = ({ numberOfIncompleteTasks }) => {
    return (
        <BrowserRouter>
          <TodoFooter 
            numberOfIncompleteTasks={numberOfIncompleteTasks}
          />
        </BrowserRouter>
    )
}

it('should render the correct amount of incomplete tasks', () => {
    render(
        <MockTodoFooter 
          numberOfIncompleteTasks={5}
        />
    );
    const pElement = screen.getByText(/5 tasks left/i);
    expect(pElement).toBeInTheDocument();
});

it('should render "task" when the number of incomplete tasks is one', () => {
  render(
      <MockTodoFooter 
        numberOfIncompleteTasks={1}
      />
  );
  const pElement = screen.getByText(/1 task left/i);
  expect(pElement).toBeInTheDocument();
});
```

## Blocs Describe

Il est possible de groupe des tests dans un bloc `describe`

```Javascript
describe("TodoFooter", () )> {
    // Tests
}
```

## Test des évenements

