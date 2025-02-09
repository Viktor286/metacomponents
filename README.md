# Schema based frontend framework
```
npm i
npm start
```
Go to `http://localhost:8080` and play with simple form.
## Features:
- Pure JavaScript only - no need to switch between HTML/CSS/JS syntax
- No 3rd party dependencies
- Tiny JIT compiler
- Dynamic module resolutions
- Declarative schema based definitions
- Component elements
- Component/element encapsulation
- Element dependency injection
- Element change detection
- Element local state
- Element lifecycle hooks
- Separate domain logics

## Form component example
### Component definition
```js
export default () => ({
  main: 'container',
  elements: {
    container: '/app/components/form/elements/container.js',
    input: '/app/components/form/elements/input.js',
    button: '/app/components/form/elements/button.js',
    buttonStyles: '/app/components/form/elements/button-styles.js'
  },
  domain: {
    sender: '/app/domain/sender.js'
  }
});
```
### Form container element
```js
const formTitle = {
  tag: 'p',
  text: 'Type your name: '
};

export default ({ elements, domain }) => ({
  tag: 'div',
  styles: {
    padding: '20px',
    backgroundColor: 'grey',
    color: 'white'
  },
  hooks: {
    init() {
      console.log('Container init');
    }
  },
  children: [
    formTitle,
    elements.input(formTitle, domain.sender),
    elements.button(elements.buttonStyles(), domain.sender)
  ]
});
```
### Form input element
```js
export default (formTitle, sender) => ({
  tag: 'input',
  styles: {
    padding: '5px 10px',
    borderRadius: '5px',
    border: 'none'
  },
  attrs: {
    type: 'text',
    placeholder: 'Name'
  },
  state: {},
  events: {
    input(event) {
      const value = event.target.value;
      sender.setData(value);
      formTitle.text = this.state.initialFormTitle + value;
    }
  },
  hooks: {
    init() {
      console.log('Input init');
      this.state.initialFormTitle = formTitle.text;
    }
  }
});
```
### Form button element
```js
export default (styles, sender) => ({
  tag: 'button',
  text: 'Send',
  styles,
  attrs: {
    name: 'sendBtn'
  },
  state: {
    count: 0
  },
  events: {
    async click() {
      this.state.count++;
      await sender.send();
      console.log(`Button clicked ${this.state.count} times`);
    }
  },
  hooks: {
    init() {
      console.log('Button init');
    }
  }
});
```
