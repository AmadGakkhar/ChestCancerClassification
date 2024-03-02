# Utility Functions

Created src/EndtoEndDeployment/utils/common.py to write some common utility functions.

*ConfigBox provides easy way to access dict elements. For Example for a dict d = {"key" : "value", "key1" : "value1"}, d.key will give error but for d2 = ConfigBox({"key" : "value", "key1" : "value1"}), d2.key will work.

** @ensure_annotation decorator enforces the data types of a function. i.e if any other data type is encountered it will make sure that an error is raised.