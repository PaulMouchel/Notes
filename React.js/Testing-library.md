# React Testing Library

Ce document est créé à partir des exemple de la série React Testing Library Tutorial de la chaine The Net Ninja
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

Test d'un composant input

```Javascript
import { render, screen, fireEvent } from '@testing-library/react';
import AddInput from "../AddInput"

const mockedSetTodo = jest.fn();

it('should render input element', () => {
    render(
        <AddInput 
            todos={[]}
            setTodos={mockedSetTodo}
        />
    );
    const inputElement = screen.getByPlaceholderText(/Add a new task here.../i);
    expect(inputElement).toBeInTheDocument();
});

it('should be able to type into input', () => {
    render(
        <AddInput 
            todos={[]}
            setTodos={mockedSetTodo}
        />
    );
    const inputElement = screen.getByPlaceholderText(/Add a new task here.../i);
    fireEvent.click(inputElement)
    fireEvent.change(inputElement, { target: { value: "Go Grocery Shopping" } })
    expect(inputElement.value).toBe("Go Grocery Shopping");
});

it('should be able to type into input', () => {
    render(
        <AddInput 
            todos={[]}
            setTodos={mockedSetTodo}
        />
    );
    const inputElement = screen.getByPlaceholderText(/Add a new task here.../i);
    fireEvent.click(inputElement)
    fireEvent.change(inputElement, { target: { value: "Go Grocery Shopping" } });
    const buttonElement = screen.getByRole("button", { name: /Add/i});
    fireEvent.click(buttonElement)
    expect(mockedSetTodo).toBeCalled()
});

it('should have empty input when add button is cliked', () => {
    render(
        <AddInput 
            todos={[]}
            setTodos={mockedSetTodo}
        />
    );
    const inputElement = screen.getByPlaceholderText(/Add a new task here.../i);
    fireEvent.change(inputElement, { target: { value: "Go Grocery Shopping" } });
    const buttonElement = screen.getByRole("button", { name: /Add/i});
    fireEvent.click(buttonElement)
    expect(inputElement.value).toBe("")
});
```
## Création d'un test d'integration

Le principe est le même, mais on test un composant parent

```Javascript
import { render, screen, fireEvent } from '@testing-library/react';
import Todo from "../Todo"
import { BrowserRouter } from "react-router-dom"

const MockTodo = () => {
    return (
        <BrowserRouter>
          <Todo/>
        </BrowserRouter>
    )
}

const addTask = (tasks) => {
    const inputElement = screen.getByPlaceholderText(/Add a new task here.../i);
    const buttonElement = screen.getByRole("button", { name: /Add/i} );
    tasks.forEach((task) => {
        fireEvent.change(inputElement, { target: { value: task } });
        fireEvent.click(buttonElement);
    })
}

it('should be able to type into input', () => {
    render(
        <MockTodo />
    );
    addTask(["Go Grocery Shopping"])
    const divElement = screen.getByText(/Go Grocery Shopping/i);
    expect(divElement).toBeInTheDocument()
});

it('should render multiple items', () => {
    render(
        <MockTodo />
    );
    addTask(["Go Grocery Shopping", "Go Grocery Shopping", "Go Grocery Shopping"])
    const divElements = screen.queryAllByText(/Go Grocery Shopping/i);
    expect(divElements.length).toBe(3)
});

it('task should not have complete class when initally rendered', () => {
    render(
        <MockTodo />
    );
    addTask(["Go Grocery Shopping"])
    const divElement = screen.getByText(/Go Grocery Shopping/i);
    expect(divElement).not.toHaveClass("todo-item-active")
});

it('task should have complete class when clicked', () => {
    render(
        <MockTodo />
    );
    addTask(["Go Grocery Shopping"])
    const divElement = screen.getByText(/Go Grocery Shopping/i);
    fireEvent.click(divElement)
    expect(divElement).toHaveClass("todo-item-active")
});
```

## Tests avec données asynchrones (fetch data dans useEffect)

sous le dossier `src`, creer un nouveau dossier `__mocks__`

Pour simuler le comportement d'axios (par exemple), on créé un fichier axios.js (sous /__mocks__)

```Javascript
const mockResponse = {
    data: {
        results: [
            {
                name: {
                    fist: "Laith",
                    last: "Harb"
                },
                picture: {
                    large: "https://randomuser.me/api/portraits/men/39.jpg"
                },
                login: {
                    username: "ThePhonyGOAT"
                }
            }
        ]
    }
}

export default {
    get: jest.fn()
}
```

Pour que les test passent, il mettre dans package.json :

```json
"jest": {
    "collectCoverageFrom": [
      "src/**/*.{js,jsx,ts,tsx}"
    ],
    "resetMocks": false
}
```
On peut esuite écrire ses tests:

```Javascript
import { render, screen, fireEvent } from '@testing-library/react';
import { BrowserRouter } from 'react-router-dom';
import FollowersList from "../FollowersList";

const MockFollowersList = () => {
    return (
        <BrowserRouter>
            <FollowersList />
        </BrowserRouter>
    )
}

describe("FollowersList", () => {

    beforeEach(() => {
        jest.mock("../../../__mocks__/axios")
    })

    it('should fetch and render input element', async () => {
        render(
            <MockFollowersList />
        );
        const followerDivElement = await screen.findByTestId(`follower-item-0`)
        expect(followerDivElement).toBeInTheDocument();
    });
    
    it('should fetch and render input element', async () => {
        render(
            <MockFollowersList />
        );
    
        const followerDivElement = await screen.findByTestId(`follower-item-0`)
        screen.debug()
        expect(followerDivElement).toBeInTheDocument();
    });
})
```

## Executer du code avant/après les tests

```Javascript
import { render, screen, fireEvent } from '@testing-library/react';
import { BrowserRouter } from 'react-router-dom';
import FollowersList from "../FollowersList";

const MockFollowersList = () => {
    return (
        <BrowserRouter>
            <FollowersList />
        </BrowserRouter>
    )
}

describe("FollowersList", () => {

    beforeEach(() => {
        console.log("RUNS BEFORE EACH TEST")
        jest.mock("../../../__mocks__/axios")
    })

    beforeAll(() => {
        console.log("RUNS ONCE BEFORE ALL TESTS")
    })

    afterEach(() => {
        console.log("RUNS AFTER EACH TEST")
    })

    afterAll(() => {
        console.log("RUNS ONCE AFTER ALL TESTS")
    })

    it('should fetch and render input element', async () => {
        render(
            <MockFollowersList />
        );
        const followerDivElement = await screen.findByTestId(`follower-item-0`)
        expect(followerDivElement).toBeInTheDocument();
    });
    
    it('should fetch and render input element', async () => {
        render(
            <MockFollowersList />
        );
    
        const followerDivElement = await screen.findByTestId(`follower-item-0`)
        expect(followerDivElement).toBeInTheDocument();
    });
})
```