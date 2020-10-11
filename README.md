# directus-conditional-fields
Directus interface for conditional fields.

This interface can be used to show any fields based on the value of the other field.
Currently **dropdown**, **radio button** and **button group** are the supported fields. Based on value of these fields you can show or hide the other fields.

#### **Note:** Only text values are supported, number only values will not work.

## How to use

Copy the contents of dist folder from this repository into `public/extensions/custom/interfaces/directus-conditional-fields` of your directus installation.
Create a **conditional-fields-list** field with field name starting with **conditional_** like conditional_type or conditional_role.

In options tab you will see a JSON input with name conditions. This is where you can configure your conditions in json format.

#### Conditions format:
Below is an example JSON condition. Where `type` is the name of the dropdown or radio button or button group. When the value is selected as `grocery` then the fields with name `weight` and `price` will be visible. If `food` is selected then `calorie` field will be visible.

When `in_grams` is set to value `yes` then `gram` field will be visible.

You can add any number of fields in array to show and also check for any number of values.

When `in_grams` is set to value `yes` then `gram` field will be visible.

```{
  "type": {
    "grocery": [
      "weight", "price"
    ],
    "food": [
      "calorie"
    ]
  },
  "in_grams": {
    "yes": [
      "grams"
    ]    
  }
}
