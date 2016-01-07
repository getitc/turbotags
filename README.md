## Format
TurboTags uses an INI flag file format, designed to be easily readable by any text editor, word processor, database or spreadsheet.  Each file will have information similar to that shown below.  The order of the tags is not important.  Any tags that are not valid will simply be ignored. The first line of this sample may appear and it will always appear exactly as shown. It is optional but gives basic format information about the information that follows.

```
"tagname", "scope", "value1", "value2", "value3", "value4"
"integformat", "sys0", "turbotags2"
"integvers", "sys0", "2", "0"
"stateabbr", "sys0", "IL"
"companyname", "pol0", "Drive – Progressive"
"companyeffdate", "pol0", "07/28/2005"
"companyphone", "pol0", "(877) 776-2436"
"term", "pol0", "6"
"numofcars", "pol0", "1"
"numofdrivers", "pol0", "1"
"liablimits1", "car1", "20"
"liablimits2", "car1", "40"
"liablimits3", "car1", "15"
"dob", "drv1", "05/23/1971"
```

You will notice that the tags are divided into common scope use groups.  Tags may have the same name and meaning, but be for a different scope.  Look up the tag example that applies to the scope for which you intend to use the data.

## Fields
Each line is divided into fields.  Each field is enclosed within double quotes, and each field is separated from the next by a comma.  No field may have a double quote within it.  The double quote is valid only as a delimiter.

#### tagname
This is the first field on each line.  It is used to determine what information is being passed on that line. Tag names are separated into seven categories based upon scope.

1. [System Tags](https://github.com/getitc/turbotags/wiki/System-Tags)
2. [Policy Tags](https://github.com/getitc/turbotags/wiki/Policy-Tags)
3. [Driver Tags](https://github.com/getitc/turbotags/wiki/Driver-Tags)
4. [Car Tags](https://github.com/getitc/turbotags/wiki/Car-Tags)
5. [Miscellaneous Premium Level Tags](https://github.com/getitc/turbotags/wiki/Miscellaneous-Premium-Level-Tags)
6. [Violation Tags](https://github.com/getitc/turbotags/wiki/Violation-Tags)
7. [Rate Engine Tags](https://github.com/getitc/turbotags/wiki/Rate-Engine-Tags)

##### scope
This is the second field on each line.  It shows the scope of the information being passed on that line.  Valid scopes are: "sys0", "pol0", "drvX", "carX", "mprX", "vioX", "useX" and "excX".

Scope|Definition
---|---
sys0|This scope denotes system information as defined in [System Tags](https://github.com/getitc/turbotags/wiki/System-Tags).
pol0|This scope denotes policy information as defined in [Policy Tags](https://github.com/getitc/turbotags/wiki/Policy-Tags).
drvX|This scope denotes driver specific information as defined in [Driver Tags](https://github.com/getitc/turbotags/wiki/Driver-Tags).  The 'X' will be a number in the range 1-6, and indicates the specific driver to whom the information refers.
carX|This scope denotes vehicle specific information as defined in [Car Tags](https://github.com/getitc/turbotags/wiki/Car-Tags).  The 'X' will be a number in the range 1-6, and indicates the specific vehicle to which the information refers. 
mprX|This scope denotes miscellaneous premium information as defined in [Miscellaneous Premium Level Tags](https://github.com/getitc/turbotags/wiki/Miscellaneous-Premium-Level-Tags).  The 'X' will be a number greater than or equal to one (1), and indicates the specific miscellaneous premium to which the information refers.
vioX|This scope denotes violation information as defined in [Violation Tags](https://github.com/getitc/turbotags/wiki/Violation-Tags).  The 'X' will be a number greater than or equal to one (1), and indicates the specific violation to which the information refers.  Violation information will also have a driver scope, to indicate which driver is the "owner" of the violation.
useX|This scope denotes usage information.  The 'X' will be a number in the range 1-6, and indicates the specific usage to which the information refers.
excX|This scope denotes excluded driver information.  The 'X' will be a number in the range 1-6, and indicates the specific excluded driver to whom the information refers.

The "sys0" and "pol0" are used unscoped.  "useX" and "excX" can be used to scope any "carX" and "drvX" tags, respectively.  The majority of the scopes you use will be "pol0", "drvX", "carX" and "vioX".

##### value1 through value4
These fields are values.  Valid values will vary, depending upon the tag name.  Not all tags will have a "value2", "value3" and/or "value4" tag.  As a general rule, violations, discounts and surcharges are the only tags which will use the "value2", "value3" and/or "value4" fields.

*Special Note on [Boolean](https://github.com/getitc/turbotags/wiki/Custom-Types#boolean) types. The type of Boolean is Y for TRUE and N for FALSE.*

## Program vs. Company Level Tags
There is a standard which needs to be noted regarding the naming of the TurboTags.  You will notice that some policy related tags have a twin with the "CO" prefix.  For example, "liabbilimits" and "coliabbilimits".  Both of these tags relate to the liability BI limits of the policy.  Because a comparative rater must sometimes bump selections to qualify a quote for a certain company, we use co-variables to bridge and work with what is actually being used for the company rather than what was selected in the comparative interface.  For example, if an agent selects a 12-month term in the comparative rater, and there is an installed company that only has 6-month policies.  If the rater is set to bump values, the bridge file will contain the following tags:

```
"term", "pol0", "12"
"coterm", "pol0", "6"
```

This tells the program receiving the bridge that, while a 12-month term was originally requested, the quote is for and was rated with a 6-month term.  Handle accordingly.  Hence forth, this document will mean the amount the policy was actually rated with, or the "CO" amount, when it refers to "rated."

## Combined Policy Notes
Some of states support "Combined Coverage," a feature that allows agents to combine a liability policy from one company with a physical damage policy from another company.  If the agent rates such a policy, and then selects the bridge option, a single bridge file will be created that contains the information for both the liability and physical damage policies.  The information for the liability policy will be contained in tags that are prefaced by the word 'secondary'. 

The financing information contained in the bridge file is that for the entire policy.  This means that the finance information tags contain the values that result from financing the combined total premiums of both the liability and physical damage policies.  To illustrate, let’s say that the agent has rated a "Combined Coverage" policy, which produced a liability policy with a total premium of $175, and a physical damage policy with a total premium of $660 (for this example, we will assume that there are no non-financed fees).  The finance company selected has a down payment of 20%.  The value in the down payment tag in the bridge file would be (175 + 660) = 835 x 0.20 = $167.
