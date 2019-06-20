# react-native-swipe-out

![swipeout preview](https://i.imgur.com/8kb5lIJ.gif)

## Installation
Just copy the source code and create a `Swipeout` folder in your components folder.

## Dependencies
1. `prop-types@^15.6.2`
2. `create-react-class@^15.6.3`
3. `react-tween-state@^0.1.5`

## Usage example

```js
import React from 'react';
import { View, Text, TouchableOpacity, ListView, StyleSheet } from 'react-native';
import Swipeout from '@components/Swipeout';

class SwipeoutExample extends React.PureComponent {
  state = {
    dataSource: null,
    sectionID: null,
    rowID: null
  }

  componentDidMount() {
    const ds = new ListView.DataSource({
      rowHasChanged: (r1, r2) => r1 !== r2,
      sectionHeaderHasChanged: (s1, s2) => s1 !== s2,
    });

    // response = array of data
    const dataSource = this.convertRowArrayToMap(response);
    this.setState({
      dataSource: ds.cloneWithRowsAndSections(dataSource),
    });
  }

  convertRowArrayToMap = data => {
    var rowCategoryMap = {}; // Create the blank map
    data.forEach(rowItem => {
      if (!rowItem.deleted) {
        rowItem.deleted = false;
      }

      const created = moment(rowItem.created).format('DD MMM YYYY, dddd');
      if (!rowCategoryMap[created]) {
        rowCategoryMap[created] = [];
      }

      if (!rowItem.deleted) {
        rowCategoryMap[created].push({
          ...rowItem,
          deleted: false,
          autoClose: false,
          backgroundColor: '#F9F3E0',
          right: [
            {
              backgroundColor: '#F9F3E0',
              component: (
                <View
                  style={{
                    justifyContent: 'center',
                    alignItems: 'center',
                    top: 20,
                    left: 10,
                    height: 40,
                    width: 40,
                    borderRadius: 300,
                    backgroundColor: '#6872E6',
                  }}
                >
                  {YOUR_EDIT_ICON_HERE}
                </View>
              ),
              onPress: () => {
                // edit
              },
            },
            {
              backgroundColor: '#F9F3E0',
              component: (
                <View
                  style={{
                    justifyContent: 'center',
                    alignItems: 'center',
                    top: 20,
                    left: 10,
                    height: 40,
                    width: 40,
                    borderRadius: 300,
                    backgroundColor: '#EB5F55',
                  }}
                >
                  {YOUR_DELETE_ICON_HERE}
                </View>
              ),
              onPress: () => {
                // delete
              },
            },
          ],
        });
      }
    });
  
  renderRow(rowData, sectionID, rowID) {
    return (
      <Swipeout
        close={
          !(this.state.sectionID === sectionID && this.state.rowID === rowID)
        }
        buttonWidth={60}
        right={rowData.right}
        rowID={rowID}
        sectionID={sectionID}
        autoClose
        backgroundColor={rowData.backgroundColor}
        onOpen={(sectionID, rowID, direction) => {
          if (direction === 'right') {
            this.setState({ isRightOpen: true, rowID, sectionID });
          }
        }}
        onClose={() => this.setState({ isRightOpen: false, rowID: null, sectionID: null })}
        openRight={this.state.isRightOpen && rowID === this.state.rowID && sectionID === this.state.sectionID}
      >
        <TouchableOpacity onPress={() => this.setState({ isRightOpen: true, rowID, sectionID })}>
          <View>
            <Text>{rowData.message}</Text>
          </View>
        </TouchableOpacity>
      </Swipeout>
    );
  }
  
  render() {
    return (
      <ListView
        scrollEnabled
        dataSource={dataSource}
        renderRow={this.renderRow.bind(this)}
        renderSectionHeader={this.renderSectionHeader}
        style={styles.listView}
      />
    )
  }
}

const styles = StyleSheet.create({
  listView: {
    flex: 1
  }
});
```

## Props

Prop            | Type   | Optional | Default   | Description
--------------- | ------ | -------- | --------- | -----------
autoClose       | bool   | Yes      | false     | auto close on button press
backgroundColor | string | Yes      | '#dbddde' |
close           | bool   | Yes      |           | close swipeout
disabled        | bool   | Yes      |  false    | whether to disable the swipeout  
left            | array  | Yes      | []        | swipeout buttons on left
onOpen          | func   | Yes      |           | (sectionID, rowId, direction: string) => void
onClose          | func   | Yes      |           | (sectionID, rowId, direction: string) => void
right           | array  | Yes      | []        | swipeout buttons on right
scroll          | func   | Yes      |           | prevent parent scroll
style           | style  | Yes      |           | style of the container
sensitivity     | number | Yes      | 50         | change the sensitivity of gesture
buttonWidth     | number | Yes      |            | each button width

##### Button props

Prop            | Type   | Optional | Default   | Description
--------------- | ------ | -------- | --------- | -----------
backgroundColor | string | Yes      | '#b6bec0' | background color
color           | string | Yes      | '#ffffff' | text color
component       | ReactNode | Yes      | null      | pass custom component to button
onPress         | func   | Yes      | null      | function executed onPress
text            | string | Yes      | 'Click Me'| text
type            | string | Yes      | 'default' | default, delete, primary, secondary
underlayColor   | string | Yes      | null      | button underlay color on press
disabled        | bool   | Yes      | false     | disable button

## Issues
[https://github.com/rjuevesano/react-native-swipe-out/issues](https://github.com/rjuevesano/react-native-swipe-out/issues)