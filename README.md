# Steeleye-Assignment
I have uploaded steel eye assignment

<h3>Q1) Explain what the simple List component does</h3>

```
Ans) The List component accepts an array of objects as props. Each object in this list has a text property. 
The List component renders the value of the text property of all the elements present in the list prop as an unordered list. 
By default, each element of the unordered list has a red background which changes to green when the element is clicked. 
This behavior might be used to tell the user that a particular element is being selected. Only one element can be selected at a time. 
Only the last clicked element should be green, and the rest of the elements should turn red.
```

<h3>Q2) What problems / warnings are there with code?</h3>

```
Ans)The warnings or problems in this code are as mentioned below:

Error1)
When we use sueState its take two perimeters. first is variable and second is a function. So we need to change this :

const [setSelectedIndex, selectedIndex] = useState();

It should be like this,

const [selectedIndex, setSelectedIndex] = useState();

Error2)
The calling functionality is wrong in the
WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shapeOf({
    text: PropTypes.string.isRequired,
  })),
};
It should be like this,

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};
Error3)
We can not map null, if we face this issues we can handle it manually, if data null or something then the page error :
WrappedListComponent.defaultProps = {
  items: null,
};
It should be like this,

WrappedListComponent.defaultProps = {
  items: [
    { index: 1, Text: "jhone" },
    { index: 2, Text: "smith" },
  ],
};

Error4)
Missing dynamic key for mapping, isSelected given a Boolean error handling for null or undefined data, it gives us better error handle and gives better user experience :
{
  items.map((item, index) => (
    <SingleListItem
      onClickHandler={() => handleClick(index)}
      text={item.text}
      index={index}
      isSelected={selectedIndex}
    />
  ));
}
It should be like this,

{
  Array.isArray(items)
    ? items?.map((items, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={Boolean(selectedIndex)}
        />
      ))
    : null;
}
```

<h3>Q3) Please fix, optimize, and/or modify the component as much as you think is necessary</h3>
Ans) Corrected Code

```
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "green" : "red" }}
      onClick={onClickHandler(index)}
    >
      {text}
    </li>
  );
};
WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};
const SingleListItem = memo(WrappedSingleListItem);
const WrappedListComponent = ({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState();
  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);
  console.log(selectedIndex);
  const handleClick = (index) => {
    setSelectedIndex(index);
  };
  return (
    <ul style={{ textAlign: "left" }}>
      {/* we can handle error here. if, when mount the component then data is
      undefine or null then app gives us null, when it gives us null or something we can show an spinner */}
      {Array.isArray(items)
        ? items?.map((item, index) => (
            <SingleListItem
              key={index}
              onClickHandler={() => handleClick(index)}
              text={item.text}
              index={index}
              isSelected={Boolean(selectedIndex)}
            />
          ))
        : null}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};
WrappedListComponent.defaultProps = {
  items: [
    { Index: 1, Text: "jhone" },
    { Index: 2, Text: "smith" },
  ],
};
const List = memo(WrappedListComponent);
export default List;
```
