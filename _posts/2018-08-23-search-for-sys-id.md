---
author: LKH
title: "Search for a specific sys_id in all tables"
---

Sometimes you get a sys_id of a record, but you do not know in which table this record is located.

When faced with this challange I often turn to a script that was provided to me by a ServiceNow support engineer named **Shahid Shah**. 

```javascript
searchIt('62d9170d6fe9a1002c8e4035eb3ee480');

function searchIt(sys_id) {
    gs.print('Searching tables for ' + sys_id);
    var baseTables = new GlideRecord('sys_db_object');
    baseTables.addEncodedQuery('super_classISEMPTY^nameNOT LIKEts_c_^nameNOT LIKEsysx_^nameNOT LIKEv_');
    baseTables.query();
    while (baseTables._next()) {
        var tableName = baseTables.name;
        var sd = new GlideRecord('sys_dictionary');
        sd.addQuery('name', tableName);
        sd.addQuery('element', 'sys_id');
        sd.queryNoDomain();
        if(!sd.next()) 
            continue;
        var current = new GlideRecord(tableName);
        current.addQuery('sys_id', sys_id);
        current.queryNoDomain();
        if(current._next()) {
            gs.print('Found it in ' + current.getClassDisplayValue() + ' [' + current.getRecordClassName() + '] <a href="' + current.getLink() + '" target="_blank" >Link</a>');
            break;
        }
    }
    gs.print('End of Search');
}
```

You can run the above as a background script and simply insert the sys_id that you are looking for in the **searchIt** function and it will output the location of the record, if it exists.