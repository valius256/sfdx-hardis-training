<!-- This file is auto-generated. if you do not want it to be overwritten, set TRUE in the line below -->
<!-- DO_NOT_OVERWRITE_DOC=FALSE -->

# Installation Close Check

## Flow Diagram

```mermaid
%% If you read this, your Markdown visualizer does not handle MermaidJS syntax.
%% - If you are in VS Code, install extension `Markdown Preview Mermaid Support` at https://marketplace.visualstudio.com/items?itemName=bierner.markdown-mermaid
%% - If you are using sfdx-hardis, try to define env variable `MERMAID_MODES=cli,docker` ,then run again the command to regenerate markdown with SVG images.
%% - If you are within a Zensical site, define the mermaid custom fence in `mkdocs.yml` as described in https://zensical.org/docs/
%% - As a last resort, you can copy-paste this MermaidJS code into https://mermaid.live/ to see the flow diagram

flowchart TB
START(["START<br/><b>AutoLaunched Flow</b></br>Type: <b>Record Before Save</b>"]):::startClass
click START "#general-information" "1548911457"

Log_Fault[\"🟰 <em></em><br/>Log Fault"/]:::assignments
click Log_Fault "#log_fault" "4127707369"

Checklist_Incomplete("🚫 <em></em><br/>Checklist Incomplete"):::customErrors
click Checklist_Incomplete "#checklist_incomplete" "3372183506"

Install_Date_Missing("🚫 <em></em><br/>Install Date Missing"):::customErrors
click Install_Date_Missing "#install_date_missing" "426048938"

Any_Item_Open{"🔀 <em></em><br/>Any Item Open"}:::decisions
click Any_Item_Open "#any_item_open" "584739389"

No_Install_Date{"🔀 <em></em><br/>No Install Date"}:::decisions
click No_Install_Date "#no_install_date" "260940220"

Get_Open_Item[("🔍 <em></em><br/>Get Open Item")]:::recordLookups
click Get_Open_Item "#get_open_item" "26115844"

Log_Fault --> END_Log_Fault
Checklist_Incomplete --> END_Checklist_Incomplete
Install_Date_Missing --> END_Install_Date_Missing
Any_Item_Open --> |"Item open"| Checklist_Incomplete
Any_Item_Open --> |"Checklist complete"| END_Any_Item_Open
No_Install_Date --> |"Date missing"| Install_Date_Missing
No_Install_Date --> |"Install date set"| Get_Open_Item
Get_Open_Item --> Any_Item_Open
Get_Open_Item -. Fault .->Log_Fault
START -->  No_Install_Date
END_Log_Fault(( END )):::endClass
END_Checklist_Incomplete(( END )):::endClass
END_Install_Date_Missing(( END )):::endClass
END_Any_Item_Open(( END )):::endClass


classDef actionCalls fill:#D4E4FC,color:black,text-decoration:none,max-height:100px
classDef assignments fill:#FBEED7,color:black,text-decoration:none,max-height:100px
classDef collectionProcessors fill:#F0E3FA,color:black,text-decoration:none,max-height:100px
classDef customErrors fill:#FFE9E9,color:black,text-decoration:none,max-height:100px
classDef decisions fill:#FDEAF6,color:black,text-decoration:none,max-height:100px
classDef loops fill:#FDEAF6,color:black,text-decoration:none,max-height:100px
classDef recordCreates fill:#FFF8C9,color:black,text-decoration:none,max-height:100px
classDef recordDeletes fill:#FFF8C9,color:black,text-decoration:none,max-height:100px
classDef recordLookups fill:#EDEAFF,color:black,text-decoration:none,max-height:100px
classDef recordRollbacks fill:#FFF8C9,color:black,text-decoration:none,max-height:100px
classDef recordUpdates fill:#FFF8C9,color:black,text-decoration:none,max-height:100px
classDef screens fill:#DFF6FF,color:black,text-decoration:none,max-height:100px
classDef subflows fill:#D4E4FC,color:black,text-decoration:none,max-height:100px
classDef startClass fill:#D9F2E6,color:black,text-decoration:none,max-height:100px
classDef endClass fill:#F9BABA,color:black,text-decoration:none,max-height:100px
classDef transforms fill:#FDEAF6,color:black,text-decoration:none,max-height:100px


```

<!-- Flow description -->

## General Information

|<!-- -->|<!-- -->|
|:---|:---|
|Object|Installation__c|
|Process Type|Auto Launched Flow|
|Trigger Type|Record Before Save|
|Record Trigger Type|Update|
|Label|Installation Close Check|
|Status|Active|
|Does Require Record Changed To Meet Criteria|✅|
|Description|An installation cannot be closed without an install date, or while its handover checklist has an item not done.|
|Environments|Default|
|Interview Label|Installation Close Check {!$Flow.CurrentDateTime}|
|BuilderType (PM)|LightningFlowBuilder|
|CanvasMode (PM)|AUTO_LAYOUT_CANVAS|
|Connector|[No_Install_Date](#no_install_date)|
|Next Node|[No_Install_Date](#no_install_date)|


#### Filters (logic: **and**)

|Filter Id|Field|Operator|Value|
|:-- |:-- |:--:|:--: |
|1|Status__c|Equal To|Completed|


## Variables

|Name|Data Type|Is Collection|Is Input|Is Output|Object Type|Description|
|:-- |:--:|:--:|:--:|:--:|:--:|:--  |
|faultMessage|String|⬜|⬜|⬜|<!-- -->|The error a failed lookup reported.|


## Flow Nodes Details

### Log_Fault

|<!-- -->|<!-- -->|
|:---|:---|
|Type|Assignment|
|Label|Log Fault|
|Description|Keeps the fault message, so that a failed lookup leaves a trace.|


#### Assignments

|Assign To Reference|Operator|Value|
|:-- |:--:|:--: |
|faultMessage|Assign|$Flow.FaultMessage|




### Checklist_Incomplete

|<!-- -->|<!-- -->|
|:---|:---|
|Type|Custom Error|
|Label|Checklist Incomplete|
|Description|Refuses to close the installation while its checklist is incomplete.|
|Custom Error Messages|errorMessage: Complete the handover checklist before closing this installation.<br/>isFieldError: false<br/>|


### Install_Date_Missing

|<!-- -->|<!-- -->|
|:---|:---|
|Type|Custom Error|
|Label|Install Date Missing|
|Description|Refuses to close an installation that was never given an install date.|
|Custom Error Messages|errorMessage: Set the install date before closing this installation.<br/>isFieldError: false<br/>|


### Any_Item_Open

|<!-- -->|<!-- -->|
|:---|:---|
|Type|Decision|
|Label|Any Item Open|
|Description|Is any handover item of this installation still not done?|
|Default Connector Label|Checklist complete|


#### Rule Item_Open (Item open)

|<!-- -->|<!-- -->|
|:---|:---|
|Connector|[Checklist_Incomplete](#checklist_incomplete)|
|Condition Logic|and|




|Condition Id|Left Value Reference|Operator|Right Value|
|:-- |:-- |:--:|:--: |
|1|[Get_Open_Item](#get_open_item)|Is Null|False|




### No_Install_Date

|<!-- -->|<!-- -->|
|:---|:---|
|Type|Decision|
|Label|No Install Date|
|Description|Was the installation ever given an install date?|
|Default Connector|[Get_Open_Item](#get_open_item)|
|Default Connector Label|Install date set|


#### Rule Date_Missing (Date missing)

|<!-- -->|<!-- -->|
|:---|:---|
|Connector|[Install_Date_Missing](#install_date_missing)|
|Condition Logic|and|




|Condition Id|Left Value Reference|Operator|Right Value|
|:-- |:-- |:--:|:--: |
|1|$Record.Install_Date__c|Is Null|True|




### Get_Open_Item

|<!-- -->|<!-- -->|
|:---|:---|
|Type|Record Lookup|
|Object|Handover_Item__c|
|Label|Get Open Item|
|Description|Finds one handover item of this installation that is not done yet.|
|Assign Null Values If No Records Found|✅|
|Fault Connector|[Log_Fault](#log_fault)|
|Get First Record Only|✅|
|Store Output Automatically|✅|
|Connector|[Any_Item_Open](#any_item_open)|


#### Filters (logic: **and**)

|Filter Id|Field|Operator|Value|
|:-- |:-- |:--:|:--: |
|1|Installation__c|Equal To|$Record.Id|
|2|Is_Done__c|Equal To|⬜|








___

_Documentation generated from branch integration by [sfdx-hardis](https://sfdx-hardis.cloudity.com), featuring [salesforce-flow-visualiser](https://github.com/toddhalfpenny/salesforce-flow-visualiser)_