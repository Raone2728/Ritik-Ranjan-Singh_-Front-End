# Ritik-Ranjan-Singh_-Front-End

##  Component Code Review

1.Explanation of the simple List Component .
ans :This is a React component that renders a selectable list of items with two nested functional components. WrappedSingleListItem renders a single list item with a background color based on whether it is selected or not. WrappedListComponent creates a list of SingleListItem components and uses the useState and useEffect hooks to manage the state of the currently selected item. Each SingleListItem component receives props and updates the selectedIndex state variable with the index of the clicked item when clicked.

2.Explaining  problems / warnings are there with code?
a.The order of destructuring is incorrect in the useState hook declaration in the WrappedListComponent. This will cause a variable that is meant to hold the state value to instead hold a function. The fix is to swap the order of the destructuring so that the variable correctly holds the state value.

b. the propTypes definition for the items prop in the WrappedListComponent is incorrect. The PropTypes.array syntax doesn't exist and should be replaced with PropTypes.arrayOf(PropTypes.shape({...})) to correctly define an array of objects with a specific shape.

c.The "isSelected" prop passed to the SingleListItem component is expected to be a boolean, but the code is passing the selectedIndex state variable, which is an integer. 

d.The onClickHandler function passed to the SingleListItem component is not correctly implemented. The current code is directly calling the handleClick function instead of returning a function that calls handleClick with the index argument.

e. The WrappedSingleListItem component is missing a closing </li> tag, which will result in a syntax error.

f.The setSelectedIndex function in the List component should be initialized with a default value. Initializing it with null can cause issues, such as isSelected being set to true for the first item in the list because null evaluates to true.

g.The onClickHandler function passed to the SingleListItem component should be wrapped in an arrow function to prevent it from being called immediately during rendering.

# optimized/or  modified  component code
## Code

```javascript
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';


const SingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
};

SingleListItem.propTypes = {
  index: PropTypes.number.isRequired,
  isSelected: PropTypes.bool.isRequired,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const MemoizedSingleListItem = memo(SingleListItem);

const List = ({
  items,
}) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items && items.map((item, index) => (
        <MemoizedSingleListItem
          key={index}
          onClickHandler={handleClick}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  )
};

List.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};

List.defaultProps = {
  items: null,
};

export default memo(List);


```
