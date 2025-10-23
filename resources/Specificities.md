# Specificity

## Critical Rules for ods-adv-analysis

- As the widget is creating an AngularJS variable containing an array of results,
  it can be displayed through {{myData[0].field_name}} to access the first result,
  or with ng-repeat="item in myData" to iterate through all results.

**IMPORTANT - Data Structure:**
`odsAdvAnalysis` returns a **direct array** (not an object with a `.results` property).
- ✅ CORRECT: {{myData[0].field_name}} or data="myData" for odsAdvTable
- ❌ INCORRECT: {{myData.results[0].field_name}} (this was the old ods-analysis v1 format)

- ✅ Fonctions supportées (COUNT, AVG, SUM, MIN, MAX, MEDIAN, date functions)
- ❌ Fonctions NON supportées (ROUND, FLOOR, CEIL, math/string functions)
- 💡 Solutions de contournement avec filtres AngularJS
