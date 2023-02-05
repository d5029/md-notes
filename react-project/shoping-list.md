# 簡易版購物清單

```javascript title="ShoppingList.js"
import { Component } from 'react';
import ShoppingListForm from './ShoppingListForm';
import { v4 as uuidv4 } from 'uuid';

class ShoppingList extends Component {
  constructor(props) {
    super(props);
    this.state = {
      items: [{ name: 'egg', qty: '2', id: uuidv4() }],
    };
    this.addItem = this.addItem.bind(this);
  }
  addItem(item) {
    let newItem = { ...item, id: uuidv4() };
    this.setState({ items: [...this.state.items, newItem] });
  }
  render() {
    return (
      <div>
        <h1>Shopping List</h1>
        <ul>
          {this.state.items.map((item, index) => (
            <li key={index}>{` ${item.name}: ${item.qty}`}</li>
          ))}
        </ul>
        <ShoppingListForm addItem={this.addItem} />
      </div>
    );
  }
}

export default ShoppingList;
```

```javascript title="ShoppingListForm.js"
import { Component } from 'react';

class ShoppingListForm extends Component {
  constructor(props) {
    super(props);
    this.state = {
      name: '',
      qty: '',
    };
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }
  handleChange(evt) {
    this.setState({
      [evt.target.name]: evt.target.value,
    });
  }
  handleSubmit(evt) {
    evt.preventDefault();
    this.props.addItem(this.state);
    this.setState({
      name: '',
      qty: '',
    });
  }
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label htmlFor='name'>Name:</label>
        <input
          id='name'
          name='name'
          onChange={this.handleChange}
          value={this.state.name}
        />
        <label htmlFor='qty'>Quanity:</label>
        <input
          id='qty'
          name='qty'
          onChange={this.handleChange}
          value={this.state.qty}
        />
        <button>Add Item!</button>
      </form>
    );
  }
}

export default ShoppingListForm;
```
