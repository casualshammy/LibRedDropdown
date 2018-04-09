# LibRedDropdown  
LibRedDropdown is another library that allows devs to add some common GUI elements into their addons. Initially this lib was created to replace Blizzard's UIDropDownMenu; now it contains 7 controls.
# API
## General notes
Getting lib instance:
```
local libRedDropdown = LibStub("LibRedDropdown -1.0");
```
All element constructors are static, you should call it like `lib.CreateControl()` (using dot). All element methods are instance methods, you should call it like `element:SomeMethod()` (using colon).  
Basically, you should call `:SetParent` and `:SetPoint` methods for all elements after creating.  
## DropdownMenu
Replacement for Blizzard's UIDropDownMenu.  
### Constructor:  
```
local dropdownMenu = libRedDropdown.CreateDropdownMenu();
```
### Methods:  
| **dropdownMenu:SetList(_table_ entities, _boolean_ dontUpdateInternalList)** |
|--:|

Sets list of entities. Example:  
```
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
  
| **dropdownMenu:GetButtonByText(_string_ text)** |
|--:|

Returns button with specified text on it (or `nil` if no button found). Example:  
```
local btn = dropdownMenu:GetButtonByText("Healing Surge")
```
_string text_: text to search
## CheckBox
Checkbox with clickable label 
### Constructor:  
```
local checkbox = libRedDropdown.CreateCheckBox();
```
### Methods: 
| **checkbox:SetText(_string_ text)** |
|--:|

Sets label's text. Example:  
```
checkbox:SetText("Click me!");
```
_string text_: new text of label  

| **checkbox:SetOnClickHandler(_function_ func)** |
|--:|

Sets `OnClick` handler. Example:  
```
checkbox:SetOnClickHandler(function() print("Clicked!"); end);
```
_function func_: function to execute on click
