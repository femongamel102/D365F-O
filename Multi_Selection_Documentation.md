# Multi-Selection Field Documentation

This document explains the implementation of a multi-selection control that allows selecting multiple values from a lookup field in a form class.

## Code Explanation

```cpp
// Public method to define a multi-selection control that can be used in the form class
// and called in the onModified event handler
public SysLookupMultiSelectCtrl multiselectCtrl;

// Override lookup method to allow selecting one or more lines from the lookup field
public void lookup() 
{
    // Create a query object
    Query query = new Query();
    QueryBuildDataSource CS_GuiltDS;
    
    // Provide the data source
    CS_GuiltDS = query.addDataSource(tableNum(CS_Guilt));
    
    // Adding fields to group by to select unique values
    CS_GuiltDS.addSortField(fieldNum(CS_Guilt, Guilt));
    CS_GuiltDS.orderMode(OrderMode::GroupBy);
    
    // Select the fields to display in the lookup
    CS_GuiltDS.addSelectionField(fieldNum(CS_Guilt, Guilt));
    
    // Provide the constructed query to the multi-select lookup control
    multiSelectCtrl = SysLookupMultiSelectCtrl::constructWithQuery(this.formRun(), this, query);
    
    // The values selected in the control are set in the multi-select control field
    // this.parmMultiselectCtl(multiselectCtrl);
    // CS_GuiltMultiCtrl.set(CS_GuiltMultiCtrl.getSelectedFieldValues());
}

// Method to return the multi-selection control value so it can be used in the onModified event handler
public SysLookupMultiSelectCtrl parmMultiselectCtl(SysLookupMultiSelectCtrl _multiselectCtrl = multiselectCtrl)
{
    multiselectCtrl = _multiselectCtrl;
    return multiselectCtrl;
}

// onModified event handler to insert multi-select data into the control
[FormControlEventHandler(formControlStr(CS_PossessionMainProcess, Gulity_Guilt), FormControlEventType::Modified)]
public static void Gulity_Guilt_OnModified(FormControl sender, FormControlEventArgs e) 
{
    FormRun formRun;
    SysLookupMultiSelectCtrl multiselectCtrl;
    
    // Get the form run instance
    formRun = sender.formRun();
    
    // Retrieve the multi-selection control
    multiselectCtrl = formRun.parmMultiselectCtl();
    
    // Get the current record from the data source
    CS_PossessionGulty cs_possessionGulty = formRun.dataSource(FormDataSourceStr(CS_PossessionMainProcess, CS_PossessionGulty)).cursor() as CS_PossessionGulty;
    
    // Store the selected values as a semicolon-separated string
    cs_possessionGulty.Guilt = con2Str(multiselectCtrl.getSelectedFieldValues(), ';');
}
```

---
*Generated for GitHub documentation*
