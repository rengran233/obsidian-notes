### Summary

|   |   |   |
|---|---|---|
|Feature|static Nested Class|Non-static Inner Class|
|**Instance Relationship**|**Independent.** Not tied to an outer class instance.|**Dependent.** Requires an instance of the outer class to exist.|
|**Instantiation**|new OuterClass.StaticNestedClass()|outerInstance.new InnerClass()|
|**Access to Outer Members**|Can only access static members of the outer class.|Can access all members (static and non-static) of the outer class.|
|**Primary Use Case**|**A helper class for grouping and namespacing.** Used when the nested class's logic does not depend on the state of an outer object.|**A true "part-of" relationship.** Used when the inner class needs to be tightly coupled with the state of an outer object.|