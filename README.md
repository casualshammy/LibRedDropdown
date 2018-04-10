# Table of contents
+ [About](#about) 
+ [API](#api) 
  + [General notes](#general_notes) 
  + [DropdownMenu](#dropdownmenu) 
  + [CheckBox](#checkbox) 
+ [Examples](#example1) 

<a name="about" />

# About 
LibRedDropdown is another library that allows devs to add some common GUI elements into their addons. Initially this lib was created to replace Blizzard's UIDropDownMenu; now it contains 7 controls.
<a name="api"/>

# API
<a name="general_notes"/>

## General notes
Getting lib instance:
``` lua
local libRedDropdown = LibStub("LibRedDropdown-1.0");
```
All element constructors are static, you should call it like `lib.CreateControl()` (using dot). All element methods are instance methods, you should call it like `element:SomeMethod()` (using colon).  
Basically, you should call `:SetParent` and `:SetPoint` methods for all elements after creating.  
<a name="dropdownmenu"/>

## DropdownMenu
Replacement for Blizzard's UIDropDownMenu.  
### Constructor
``` lua
local dropdownMenu = libRedDropdown.CreateDropdownMenu();
```
### Methods
| Method | Description |
|--------------------------------------------------------------------------|-----------------------------------------|
| dropdownMenu:SetList(_table_ entities, _boolean_ dontUpdateInternalList) | Sets list of entities. [Details](#example1) |
| dropdownMenu:GetButtonByText(_string_ text)                              | Returns button with specified text on it (or `nil` if no button found). [Details](#example2) |

<a name="checkbox"/>

## CheckBox
Checkbox with clickable label 
### Constructor  
```lua
local checkbox = libRedDropdown.CreateCheckBox();
```
### Methods  
| Method | Description |
|---------------------------------------------|----------------------------------------------|
| checkbox:SetText(_string_ text)             | Sets label's text. [Details](#example3)      |
| checkbox:SetOnClickHandler(_function_ func) | Sets `OnClick` handler. [Details](#example4) |

## Tri-state checkbox
Checkbox with three states: disabled, enabled#1 and enabled#2
### Constructor  
```lua
local triStateCheckbox = libRedDropdown.CreateCheckBoxTristate();
```
### Methods
| Method | Description |
|--------|-------------|
| triStateCheckbox:SetTextEntries(_table_ entries)    | Sets text values for each state. [Example](#example5)           |
| triStateCheckbox:SetTriState(_integer_ state)       | Sets state of checkbox. [Example](#example5)                    |
| triStateCheckbox:GetTriState()                      | Returns the state of checkbox (_integer_). [Example](#example5) |
| triStateCheckbox:SetOnClickHandler(_function_ func) | Sets `OnClick` handler. [Example](#example5)                    |

# Examples  
<a name="example1" />

***
### dropdownMenu:SetList(_table_ entities, _boolean_ dontUpdateInternalList)
_table entites_: it is a table with information about buttons that DropdownMenu should display. Valid fields of each entity:  
  * .text: button's text (string)  
  * .font: button's text font (string)  
  * .icon: button's icon (integer)  
  * .func: function that will be executed on user click (function)  
  * .onEnter: function that will be executed on mouse enter (function)  
  * .onLeave: function that will be executed on mouse leave (function)  
  * .disabled: set to true to disable button - will be grayed out (boolean)  
  * .dontCloseOnClick: set to true to prevent DropdownMenu from hiding when user clicks on button in list (boolean)  

_boolean dontUpdateInternalList_: prevents this method from changing internal list of buttons. Used internally by searchbox.  
``` lua
local t = { };
for i = 1, 100 do
  local spellName, _, spellIcon = GetSpellInfo(i);
  table.insert(t, {
    icon = spellIcon,
    text = spellName,
    onEnter = function(self)
      GameTooltip:SetOwner(self, "ANCHOR_RIGHT");
      GameTooltip:SetSpellByID(i);
      GameTooltip:Show();
    end,
    onLeave = function() GameTooltip:Hide(); end,
    func = function() print(i, spellName); end,
  });
end
dropdownMenu:SetList(t);
``` 
***
<a name="example2" />

### dropdownMenu:GetButtonByText(_string_ text) 
_string text_: text to search
```lua
local btn = dropdownMenu:GetButtonByText("Healing Surge")
```
***
<a name="example3" />

### checkbox:SetText(_string_ text) 
_string text_: new text of label 
```lua
checkbox:SetText("Click me!");
```
***
<a name="example4" />

### checkbox:SetOnClickHandler(_function_ func)
_function func_: function to execute on click
```lua
checkbox:SetOnClickHandler(function() print("Clicked!"); end);
```
***
<a name="example5" />

### Tri-state checkbox
``` lua
local triStateCheckbox = libRedDropdown.CreateCheckBoxTristate();
triStateCheckbox:SetTextEntries({
  "Disabled",
  "Enabled#1",
  "Enabled#2",
});
triStateCheckbox:SetOnClickHandler(function(self)
  if (self:GetTriState() == 0) then
    print("Disabled");
  elseif (self:GetTriState() == 1) then
    print("Enabled#1");
  else
    print("Enabled#2");
  end
end);
triStateCheckbox:SetTriState(1); -- Set to "Enabled#1"
```
***
