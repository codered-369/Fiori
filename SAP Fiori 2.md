# SAP UI5 Interview Prep - 7 Day Sprint Guide
**Complete in 1 Week | From Basics to Job-Ready**

---

## 📅 Your 7-Day Study Plan

### **Day 1: UI5 Basics & MVC (2-3 hours)**
- ✅ What is SAP UI5
- ✅ MVC Architecture
- ✅ Project Structure
- ✅ Component.js & manifest.json basics

### **Day 2: Data Binding & Controls (2-3 hours)**
- ✅ One-way, Two-way, Aggregation binding
- ✅ Common sap.m controls
- ✅ Expression binding
- ✅ Practice: Build a simple form

### **Day 3: OData & Services (2-3 hours)**
- ✅ OData Model initialization
- ✅ CRUD operations
- ✅ Filters, $select, $expand
- ✅ Error handling

### **Day 4: Routing & Navigation (2-3 hours)**
- ✅ Route configuration
- ✅ Navigation methods
- ✅ Pattern matching
- ✅ Practice: Master-detail app

### **Day 5: Fragments, Formatters & Advanced (2-3 hours)**
- ✅ Fragments & Dialogs
- ✅ Formatters
- ✅ Message handling
- ✅ Custom validation

### **Day 6: FLP & BTP Deployment (3-4 hours)**
- ✅ Fiori Launchpad configuration
- ✅ BTP basics
- ✅ MTA deployment
- ✅ Work Zone setup

### **Day 7: Review & Interview Questions (2-3 hours)**
- ✅ Review all concepts
- ✅ Practice top 50 questions
- ✅ Mock interview scenarios
- ✅ Prepare project examples

---

# DAY 1: UI5 Basics & MVC Architecture

## 🎯 Learning Goals
By end of Day 1, you should be able to:
- Explain what SAP UI5 is
- Understand MVC pattern
- Know project structure
- Create basic Component.js

---

## What is SAP UI5?

**Simple Definition:**
> SAP UI5 is a JavaScript framework for building web apps that work on desktop, tablet, and mobile.

**Key Points to Remember:**
1. HTML5-based framework
2. Follows MVC pattern
3. Provides ready-made UI controls (buttons, tables, forms)
4. Works with SAP backend systems via OData

**Remember This Visual:**

```
┌─────────────────────────────────────┐
│         SAP UI5 Framework           │
├─────────────────────────────────────┤
│  Desktop  │  Tablet  │   Mobile     │
├─────────────────────────────────────┤
│  Controls │  Data    │  Routing     │
│  (sap.m)  │  Binding │  Navigation  │
├─────────────────────────────────────┤
│         OData Backend               │
│    (SAP ABAP/Gateway/CAP)           │
└─────────────────────────────────────┘
```

---

## MVC Architecture (MOST IMPORTANT!)

**Visual Memory Aid:**

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│    VIEW      │────▶│  CONTROLLER  │────▶│    MODEL     │
│  (What you   │     │  (Brain -    │     │  (Data -     │
│   see)       │     │   Logic)     │     │   JSON/OData)│
│  .view.xml   │     │.controller.js│     │              │
└──────────────┘     └──────────────┘     └──────────────┘
     ▲                                            │
     └────────────────────────────────────────────┘
              Data flows to update view
```

### View (XML) - What you see
```xml
<mvc:View
    controllerName="com.myapp.controller.Main"
    xmlns:mvc="sap.ui.core.mvc"
    xmlns="sap.m">
    
    <Page title="Product List">
        <List items="{/products}">
            <StandardListItem
                title="{name}"
                description="{price} USD"/>
        </List>
    </Page>
</mvc:View>
```

### Controller (JS) - The brain
```javascript
sap.ui.define([
    "sap/ui/core/mvc/Controller",
    "sap/ui/model/json/JSONModel"
], function (Controller, JSONModel) {
    return Controller.extend("com.myapp.controller.Main", {
        
        onInit: function () {
            // Create data
            var oData = {
                products: [
                    {name: "Laptop", price: 1200},
                    {name: "Mouse", price: 25}
                ]
            };
            
            // Create model and set to view
            var oModel = new JSONModel(oData);
            this.getView().setModel(oModel);
        }
    });
});
```

### Model - The data
```javascript
// JSON Model (for local data)
var oModel = new JSONModel({
    userName: "John",
    age: 25
});

// OData Model (for backend data)
var oModel = new sap.ui.model.odata.v2.ODataModel("/service/url/");
```

---

## Project Structure (Memorize This!)

```
webapp/
├── 📄 Component.js          ← App entry point (IMPORTANT!)
├── 📄 manifest.json         ← App configuration (IMPORTANT!)
├── 📄 index.html           ← HTML entry
│
├── 📁 controller/
│   ├── Main.controller.js
│   └── Detail.controller.js
│
├── 📁 view/
│   ├── Main.view.xml
│   └── Detail.view.xml
│
├── 📁 model/
│   └── formatter.js        ← Helper functions
│
├── 📁 i18n/
│   └── i18n.properties     ← Translations
│
└── 📁 css/
    └── style.css           ← Custom styles
```

---

## Component.js (The Starting Point)

**What it does:**
1. Initializes the app
2. Sets up models
3. Starts routing

**Basic Template:**
```javascript
sap.ui.define([
    "sap/ui/core/UIComponent",
    "sap/ui/model/json/JSONModel"
], function (UIComponent, JSONModel) {
    "use strict";

    return UIComponent.extend("com.myapp.Component", {
        
        metadata: {
            manifest: "json"  // Use manifest.json
        },

        init: function () {
            // 1. Call parent init
            UIComponent.prototype.init.apply(this, arguments);

            // 2. Initialize router
            this.getRouter().initialize();

            // 3. Set device model
            var oDeviceModel = new JSONModel(sap.ui.Device);
            this.setModel(oDeviceModel, "device");
        }
    });
});
```

**Remember: Component.js runs BEFORE any view loads!**

---

## Controller Lifecycle (Learn the Order!)

```
1. onInit()           ← Called ONCE when controller created
        ↓
2. onBeforeRendering() ← Before view is rendered
        ↓
3. onAfterRendering()  ← After view is rendered (DOM ready)
        ↓
4. ...user interaction...
        ↓
5. onExit()           ← Cleanup when controller destroyed
```

**Example:**
```javascript
sap.ui.define([
    "sap/ui/core/mvc/Controller"
], function (Controller) {
    return Controller.extend("com.myapp.controller.Main", {
        
        onInit: function () {
            console.log("1. Controller created");
            // Initialize models here
        },

        onBeforeRendering: function () {
            console.log("2. About to render");
        },

        onAfterRendering: function () {
            console.log("3. View rendered, DOM ready");
            // Can access DOM elements here
        },

        onExit: function () {
            console.log("4. Cleanup");
            // Destroy dialogs, unbind events
        }
    });
});
```

---

## 💡 Day 1 Quick Tips

### Do's ✅
- Always use `sap.ui.define` (not `sap.ui.require`)
- Name controllers with `.controller.js`
- Name views with `.view.xml`
- Use `this.getView()` to access view from controller

### Don'ts ❌
- Don't manipulate DOM directly (use data binding)
- Don't forget to call parent `init()` in Component.js
- Don't hardcode text (use i18n)

---

## 📝 Day 1 Practice Questions

**Q1: What is SAP UI5?**
✅ Answer: HTML5-based JavaScript framework for building responsive enterprise web applications using MVC pattern.

**Q2: What does MVC stand for?**
✅ Answer: Model (data), View (UI), Controller (logic)

**Q3: What is Component.js?**
✅ Answer: The main entry point that initializes the app, sets up models, and starts routing.

**Q4: What are controller lifecycle methods?**
✅ Answer: onInit, onBeforeRendering, onAfterRendering, onExit

**Q5: What file type is used for views?**
✅ Answer: XML (most common), also JSON, HTML, JS

---

# DAY 2: Data Binding & Controls

## 🎯 Learning Goals
- Understand 3 types of data binding
- Use common sap.m controls
- Build a simple form
- Use expression binding

---

## Data Binding Types (CRITICAL!)

### Visual Memory Aid:
```
┌─────────────────┐
│  ONE-WAY        │  Model → View (display only)
│  ════════▶      │  Example: <Text text="{/name}"/>
└─────────────────┘

┌─────────────────┐
│  TWO-WAY        │  Model ⇄ View (edit & update)
│  ◀═══════▶      │  Example: <Input value="{/name}"/>
└─────────────────┘

┌─────────────────┐
│  ONE-TIME       │  Model → View (once, no updates)
│  ─────▶         │  Example: <Text text="{path:'/name', mode:'OneTime'}"/>
└─────────────────┘
```

---

## Property Binding (Simple)

```xml
<!-- Bind to a single property -->
<Text text="{/userName}"/>
<Input value="{/age}"/>
<CheckBox selected="{/isActive}"/>
```

**JavaScript Model:**
```javascript
var oModel = new JSONModel({
    userName: "John Doe",
    age: 30,
    isActive: true
});
this.getView().setModel(oModel);
```

---

## Aggregation Binding (Lists/Tables)

**Visual:**
```
Model Data (Array):          View (List):
[                            ┌──────────────┐
  {name: "A", price: 10},    │ A - $10      │
  {name: "B", price: 20},    │ B - $20      │
  {name: "C", price: 30}     │ C - $30      │
]                            └──────────────┘
```

**Code:**
```xml
<List items="{/products}">
    <StandardListItem
        title="{name}"
        description="${price}"/>
</List>
```

```javascript
// Model
var oModel = new JSONModel({
    products: [
        {name: "Laptop", price: 1200},
        {name: "Mouse", price: 25},
        {name: "Keyboard", price: 75}
    ]
});
```

---

## Expression Binding (Smart!)

**Show/Hide based on data:**
```xml
<!-- Show text only if stock > 0 -->
<Text 
    text="In Stock" 
    visible="{= ${/stock} > 0 }"/>

<!-- Change text based on price -->
<Text text="{= ${/price} > 100 ? 'Expensive' : 'Affordable' }"/>

<!-- Enable button only if name is not empty -->
<Button 
    text="Submit" 
    enabled="{= ${/name}.length > 0 }"/>
```

**Remember:** `${...}` to access model properties inside `{= ... }`

---

## Common Controls (Memorize These!)

### Input Controls
```xml
<!-- Text Input -->
<Input 
    value="{/name}" 
    placeholder="Enter name"/>

<!-- Number Input -->
<Input 
    value="{/age}" 
    type="Number"/>

<!-- Password -->
<Input 
    value="{/password}" 
    type="Password"/>

<!-- Text Area -->
<TextArea 
    value="{/description}" 
    rows="5"/>

<!-- Checkbox -->
<CheckBox 
    selected="{/isActive}" 
    text="Active"/>

<!-- Radio Button -->
<RadioButton 
    selected="{/gender}" 
    text="Male"/>

<!-- Date Picker -->
<DatePicker 
    value="{/birthDate}"/>

<!-- Dropdown -->
<Select selectedKey="{/country}">
    <items>
        <core:Item key="US" text="United States"/>
        <core:Item key="UK" text="United Kingdom"/>
    </items>
</Select>

<!-- Combobox (searchable dropdown) -->
<ComboBox selectedKey="{/city}">
    <items>
        <core:Item key="NY" text="New York"/>
        <core:Item key="LA" text="Los Angeles"/>
    </items>
</ComboBox>
```

### Display Controls
```xml
<!-- Simple Text -->
<Text text="Hello World"/>

<!-- Label -->
<Label text="User Name:" labelFor="nameInput"/>

<!-- Title -->
<Title text="Section Title" level="H2"/>

<!-- Link -->
<Link text="Click here" href="https://example.com"/>
```

### Action Controls
```xml
<!-- Button -->
<Button 
    text="Submit" 
    press=".onSubmit" 
    type="Emphasized"/>

<!-- Icon Button -->
<Button 
    icon="sap-icon://delete" 
    press=".onDelete"/>

<!-- Toggle Button -->
<ToggleButton 
    text="Show Details" 
    pressed="{/showDetails}"/>
```

### Layout Controls
```xml
<!-- Vertical Layout -->
<VBox>
    <Text text="Item 1"/>
    <Text text="Item 2"/>
</VBox>

<!-- Horizontal Layout -->
<HBox>
    <Button text="Left"/>
    <Button text="Right"/>
</HBox>

<!-- Form -->
<f:SimpleForm editable="true">
    <Label text="Name"/>
    <Input value="{/name}"/>
    
    <Label text="Email"/>
    <Input value="{/email}"/>
</f:SimpleForm>
```

---

## 🔥 Day 2 Practice: Build a Product Form

**Goal:** Create a form to add a product with validation.

```xml
<mvc:View
    controllerName="com.myapp.controller.Main"
    xmlns:mvc="sap.ui.core.mvc"
    xmlns="sap.m"
    xmlns:f="sap.ui.layout.form">

    <Page title="Add Product">
        <content>
            <f:SimpleForm>
                <f:content>
                    <Label text="Product Name"/>
                    <Input 
                        id="nameInput"
                        value="{/productName}"
                        placeholder="Enter product name"
                        valueLiveUpdate="true"
                        liveChange=".onNameChange"/>
                    
                    <Label text="Price"/>
                    <Input 
                        value="{/price}"
                        type="Number"
                        placeholder="0.00"/>
                    
                    <Label text="In Stock"/>
                    <CheckBox selected="{/inStock}"/>
                    
                    <Label text="Category"/>
                    <Select selectedKey="{/category}">
                        <items>
                            <core:Item key="Electronics" text="Electronics"/>
                            <core:Item key="Clothing" text="Clothing"/>
                            <core:Item key="Books" text="Books"/>
                        </items>
                    </Select>
                </f:content>
            </f:SimpleForm>
            
            <Button 
                text="Save Product" 
                press=".onSave"
                type="Emphasized"
                enabled="{= ${/productName}.length > 0 }"/>
        </content>
    </Page>
</mvc:View>
```

**Controller:**
```javascript
sap.ui.define([
    "sap/ui/core/mvc/Controller",
    "sap/ui/model/json/JSONModel",
    "sap/m/MessageToast"
], function (Controller, JSONModel, MessageToast) {
    return Controller.extend("com.myapp.controller.Main", {
        
        onInit: function () {
            var oModel = new JSONModel({
                productName: "",
                price: 0,
                inStock: false,
                category: "Electronics"
            });
            this.getView().setModel(oModel);
        },

        onNameChange: function (oEvent) {
            var sValue = oEvent.getParameter("value");
            var oInput = oEvent.getSource();
            
            // Validation
            if (sValue.length < 3) {
                oInput.setValueState("Error");
                oInput.setValueStateText("Minimum 3 characters");
            } else {
                oInput.setValueState("Success");
            }
        },

        onSave: function () {
            var oModel = this.getView().getModel();
            var oData = oModel.getData();
            
            MessageToast.show("Product saved: " + oData.productName);
            
            // Reset form
            oModel.setData({
                productName: "",
                price: 0,
                inStock: false,
                category: "Electronics"
            });
        }
    });
});
```

---

## 📝 Day 2 Practice Questions

**Q6: What are the 3 types of data binding?**
✅ Answer: One-way (display), Two-way (edit), One-time (initial only)

**Q7: How do you bind a list of items?**
✅ Answer: `<List items="{/arrayPath}">` (aggregation binding)

**Q8: What is expression binding?**
✅ Answer: Logic inside binding: `{= ${/price} > 100 ? 'High' : 'Low' }`

**Q9: Which namespace is used for mobile controls?**
✅ Answer: `sap.m` (xmlns="sap.m")

**Q10: How to validate input in UI5?**
✅ Answer: Use `setValueState("Error")` and `setValueStateText("message")`

---

# DAY 3: OData Services & CRUD

## 🎯 Learning Goals
- Initialize OData model
- Read, Create, Update, Delete data
- Use filters and parameters
- Handle errors

---

## What is OData?

**Simple Definition:**
> OData = REST API for SAP backend data

**Visual:**
```
┌──────────────┐     HTTP GET/POST/PUT/DELETE    ┌──────────────┐
│   UI5 App    │ ════════════════════════════▶   │  SAP Backend │
│   (Frontend) │                                  │   (OData)    │
│              │ ◀════════════════════════════    │              │
└──────────────┘     JSON/XML Response            └──────────────┘
```

**OData URLs Pattern:**
```
/sap/opu/odata/sap/SERVICE_NAME/EntitySet
                    └─────┘     └──────┘
                    Service     Entity
```

---

## Initialize OData Model

### In manifest.json:
```json
{
    "sap.app": {
        "dataSources": {
            "mainService": {
                "uri": "/sap/opu/odata/sap/ZPRODUCT_SRV/",
                "type": "OData",
                "settings": {
                    "odataVersion": "2.0"
                }
            }
        }
    },
    "sap.ui5": {
        "models": {
            "": {
                "dataSource": "mainService"
            }
        }
    }
}
```

### Or in Controller:
```javascript
var oModel = new sap.ui.model.odata.v2.ODataModel("/sap/opu/odata/sap/ZPRODUCT_SRV/");
this.getView().setModel(oModel);
```

---

## CRUD Operations (MEMORIZE!)

### ✅ CREATE (POST)
```javascript
var oModel = this.getView().getModel();

var oNewProduct = {
    ProductID: "P123",
    ProductName: "New Laptop",
    Price: "1200",
    Category: "Electronics"
};

oModel.create("/Products", oNewProduct, {
    success: function(oData) {
        sap.m.MessageToast.show("Product created!");
    },
    error: function(oError) {
        sap.m.MessageBox.error("Failed to create product");
    }
});
```

### 📖 READ (GET)
```javascript
// Read single entity
oModel.read("/Products('P123')", {
    success: function(oData) {
        console.log(oData);
    }
});

// Read all products
oModel.read("/Products", {
    success: function(oData) {
        console.log(oData.results);  // Array of products
    }
});

// Read with filters
var aFilters = [
    new sap.ui.model.Filter("Price", "GT", 100)
];

oModel.read("/Products", {
    filters: aFilters,
    success: function(oData) {
        console.log(oData.results);
    }
});
```

### ✏️ UPDATE (PUT/PATCH)
```javascript
var oUpdatedProduct = {
    ProductName: "Updated Laptop",
    Price: "1300"
};

oModel.update("/Products('P123')", oUpdatedProduct, {
    success: function() {
        sap.m.MessageToast.show("Product updated!");
    },
    error: function(oError) {
        sap.m.MessageBox.error("Update failed");
    }
});
```

### ❌ DELETE
```javascript
oModel.remove("/Products('P123')", {
    success: function() {
        sap.m.MessageToast.show("Product deleted!");
    },
    error: function(oError) {
        sap.m.MessageBox.error("Delete failed");
    }
});
```

---

## OData URL Parameters (Important!)

### Visual Reference:
```
/Products?$filter=Price gt 100&$select=ProductID,Name&$top=10&$skip=20

         └──────┘ └────────┘ └───────────────┘ └──┘ └────┘
         Filter   Condition     Select fields   Limit  Offset
```

### Common Parameters:

**$filter** - Filter results
```javascript
// Price greater than 100
"$filter": "Price gt 100"

// Category equals Electronics
"$filter": "Category eq 'Electronics'"

// Combined conditions
"$filter": "Price gt 100 and Category eq 'Electronics'"
```

**$select** - Choose specific fields
```javascript
oModel.read("/Products", {
    urlParameters: {
        "$select": "ProductID,ProductName,Price"
    }
});
```

**$expand** - Get related entities
```javascript
// Get Product with Supplier data
oModel.read("/Products('P123')", {
    urlParameters: {
        "$expand": "Supplier"
    }
});
```

**$top and $skip** - Pagination
```javascript
// Get 10 records, skip first 20
oModel.read("/Products", {
    urlParameters: {
        "$top": 10,
        "$skip": 20
    }
});
```

**$orderby** - Sort results
```javascript
urlParameters: {
    "$orderby": "Price desc"  // Descending
}
```

---

## Filter Operators (Quick Reference)

```javascript
Filter Operator Guide:
═══════════════════════
"EQ"  = Equal to
"NE"  = Not equal
"GT"  = Greater than
"GE"  = Greater or equal
"LT"  = Less than
"LE"  = Less or equal
"Contains"  = Contains text
"StartsWith" = Starts with text
```

**Examples:**
```javascript
// Single filter
var oFilter = new sap.ui.model.Filter("Price", "GT", 100);

// Multiple filters (AND)
var aFilters = [
    new sap.ui.model.Filter("Price", "GT", 100),
    new sap.ui.model.Filter("Category", "EQ", "Electronics")
];

// Multiple filters (OR)
var aFilters = [
    new sap.ui.model.Filter("Category", "EQ", "Electronics"),
    new sap.ui.model.Filter("Category", "EQ", "Clothing")
];
var oOrFilter = new sap.ui.model.Filter({
    filters: aFilters,
    and: false  // OR condition
});
```

---

## Batch Requests (Performance!)

**Why:** Combine multiple operations into 1 HTTP request

```javascript
// Enable batch
oModel.setUseBatch(true);
oModel.setDeferredGroups(["myBatchGroup"]);

// Add multiple creates
oModel.create("/Products", product1, {groupId: "myBatchGroup"});
oModel.create("/Products", product2, {groupId: "myBatchGroup"});
oModel.create("/Products", product3, {groupId: "myBatchGroup"});

// Submit all at once
oModel.submitChanges({
    groupId: "myBatchGroup",
    success: function() {
        MessageToast.show("All products created!");
    }
});
```

---

## Error Handling (Must Know!)

```javascript
oModel.read("/Products", {
    success: function(oData) {
        // Success handler
    },
    error: function(oError) {
        // Extract error message
        var sErrorMessage = "An error occurred";
        
        if (oError.responseText) {
            try {
                var oErrorResponse = JSON.parse(oError.responseText);
                sErrorMessage = oErrorResponse.error.message.value;
            } catch (e) {
                sErrorMessage = oError.message;
            }
        }
        
        sap.m.MessageBox.error(sErrorMessage);
    }
});
```

---

## 📝 Day 3 Practice Questions

**Q11: What does CRUD stand for?**
✅ Answer: Create, Read, Update, Delete

**Q12: Which method is used to create an OData entry?**
✅ Answer: `oModel.create("/EntitySet", data, {success, error})`

**Q13: What is $expand used for?**
✅ Answer: To retrieve related entities in one request

**Q14: How do you filter OData results?**
✅ Answer: Using `sap.ui.model.Filter` with operators like EQ, GT, LT

**Q15: What is batch processing?**
✅ Answer: Combining multiple OData operations into a single HTTP request

---

# DAY 4: Routing & Navigation

## 🎯 Learning Goals
- Configure routes in manifest.json
- Navigate between views
- Pass parameters
- Handle pattern matching

---

## Routing Concept (Visual)

```
User clicks → Hash changes → Router → Loads View

Example URLs:
/#/                        → Home view
/#/products                → Product List view
/#/product/123             → Product Detail (ID: 123)
/#/product/123/edit        → Edit Product
```

---

## Configure Routes in manifest.json

```json
{
    "sap.ui5": {
        "routing": {
            "config": {
                "routerClass": "sap.m.routing.Router",
                "viewType": "XML",
                "viewPath": "com.myapp.view",
                "controlId": "app",
                "controlAggregation": "pages"
            },
            "routes": [
                {
                    "pattern": "",
                    "name": "home",
                    "target": "home"
                },
                {
                    "pattern": "products",
                    "name": "productList",
                    "target": "productList"
                },
                {
                    "pattern": "product/{productId}",
                    "name": "productDetail",
                    "target": "productDetail"
                }
            ],
            "targets": {
                "home": {
                    "viewName": "Home",
                    "viewLevel": 1
                },
                "productList": {
                    "viewName": "ProductList",
                    "viewLevel": 2
                },
                "productDetail": {
                    "viewName": "ProductDetail",
                    "viewLevel": 3
                }
            }
        }
    }
}
```

**Pattern Explained:**
```
"product/{productId}"
         └────┘
      Parameter name
      (accessible in controller)
```

---

## Navigation Methods

### Navigate TO a route:
```javascript
// Simple navigation
this.getRouter().navTo("productList");

// With parameters
this.getRouter().navTo("productDetail", {
    productId: "P123"
});

// With query parameters
this.getRouter().navTo("productDetail", {
    productId: "P123"
}, false, {
    query: {
        tab: "details"
    }
});
```

### Navigate BACK:
```javascript
// Method 1: Use history
var oHistory = sap.ui.core.routing.History.getInstance();
var sPreviousHash = oHistory.getPreviousHash();

if (sPreviousHash !== undefined) {
    window.history.go(-1);
} else {
    this.getRouter().navTo("home", {}, true);
}

// Method 2: Direct to specific route
this.getRouter().navTo("productList");
```

---

## Handle Route Parameters

```javascript
sap.ui.define([
    "sap/ui/core/mvc/Controller"
], function (Controller) {
    return Controller.extend("com.myapp.controller.ProductDetail", {
        
        onInit: function () {
            // Attach to route
            var oRouter = this.getOwnerComponent().getRouter();
            oRouter.getRoute("productDetail").attachPatternMatched(this._onObjectMatched, this);
        },

        _onObjectMatched: function (oEvent) {
            // Get parameter from URL
            var sProductId = oEvent.getParameter("arguments").productId;
            
            console.log("Product ID:", sProductId);
            
            // Bind view to this product
            this.getView().bindElement({
                path: "/Products('" + sProductId + "')",
                events: {
                    dataReceived: function() {
                        // Data loaded
                    },
                    dataRequested: function() {
                        // Loading started
                    }
                }
            });
        }
    });
});
```

---

## Master-Detail Pattern (Common!)

**Visual:**
```
┌─────────────────────────────────────┐
│  Master (List)   │   Detail         │
│ ┌─────────────┐  │ ┌──────────────┐ │
│ │ Product 1   │  │ │  Product 1   │ │
│ │ Product 2   │─▶│ │  Details:    │ │
│ │ Product 3   │  │ │  Name: ...   │ │
│ └─────────────┘  │ │  Price: ...  │ │
│                  │ └──────────────┘ │
└─────────────────────────────────────┘
```

### Master View (Product List):
```xml
<List 
    items="{/Products}"
    mode="SingleSelectMaster"
    selectionChange=".onSelectionChange">
    <StandardListItem
        title="{ProductName}"
        description="{Category}"
        type="Navigation"/>
</List>
```

### Master Controller:
```javascript
onSelectionChange: function (oEvent) {
    var oSelectedItem = oEvent.getParameter("listItem");
    var oContext = oSelectedItem.getBindingContext();
    var sProductId = oContext.getProperty("ProductID");
    
    // Navigate to detail
    this.getRouter().navTo("productDetail", {
        productId: sProductId
    });
}
```

---

## App Container Structure

```xml
<!-- App.view.xml (Main Container) -->
<mvc:View
    controllerName="com.myapp.controller.App"
    xmlns:mvc="sap.ui.core.mvc"
    xmlns="sap.m">
    
    <App id="app">
        <!-- Router will load views here -->
    </App>
</mvc:View>
```

**Remember:** `controlId: "app"` in routing config must match the `id="app"` of your container!

---

## 📝 Day 4 Practice Questions

**Q16: How do you define a route with parameters?**
✅ Answer: `"pattern": "product/{productId}"` in manifest.json routes

**Q17: How to navigate to a route?**
✅ Answer: `this.getRouter().navTo("routeName", {params})`

**Q18: How to get route parameters in controller?**
✅ Answer: Attach to route pattern matched event and use `oEvent.getParameter("arguments")`

**Q19: What is the purpose of targets in routing?**
✅ Answer: Define which view to display for each route

**Q20: How to navigate back?**
✅ Answer: Check history, if exists use `window.history.go(-1)`, else navTo home

---

# DAY 5: Fragments, Formatters & Advanced

## 🎯 Learning Goals
- Create reusable fragments
- Use dialogs
- Write formatter functions
- Handle validation

---

## Fragments (Reusable UI Parts)

**What:** UI snippet without controller (can be reused)

**Visual:**
```
Main View ──┬──▶ Fragment 1 (Dialog)
            ├──▶ Fragment 2 (Form)
            └──▶ Fragment 3 (Popover)
```

### Create Fragment:
```xml
<!-- ProductDialog.fragment.xml -->
<core:FragmentDefinition
    xmlns="sap.m"
    xmlns:core="sap.ui.core">
    
    <Dialog
        title="Add Product"
        contentWidth="400px">
        <content>
            <VBox class="sapUiSmallMargin">
                <Label text="Product Name"/>
                <Input value="{/productName}"/>
                
                <Label text="Price"/>
                <Input value="{/price}" type="Number"/>
            </VBox>
        </content>
        <beginButton>
            <Button text="Save" press=".onSaveProduct"/>
        </beginButton>
        <endButton>
            <Button text="Cancel" press=".onCloseDialog"/>
        </endButton>
    </Dialog>
</core:FragmentDefinition>
```

### Load Fragment in Controller:
```javascript
sap.ui.define([
    "sap/ui/core/mvc/Controller",
    "sap/ui/core/Fragment"
], function (Controller, Fragment) {
    return Controller.extend("com.myapp.controller.Main", {
        
        onOpenDialog: function () {
            // Check if dialog exists
            if (!this._oDialog) {
                // Load fragment
                Fragment.load({
                    id: this.getView().getId(),
                    name: "com.myapp.view.ProductDialog",
                    controller: this
                }).then(function (oDialog) {
                    this._oDialog = oDialog;
                    this.getView().addDependent(this._oDialog);
                    this._oDialog.open();
                }.bind(this));
            } else {
                this._oDialog.open();
            }
        },

        onCloseDialog: function () {
            this._oDialog.close();
        },

        onSaveProduct: function () {
            // Save logic
            this._oDialog.close();
        },

        onExit: function () {
            // Cleanup
            if (this._oDialog) {
                this._oDialog.destroy();
            }
        }
    });
});
```

---

## Message Box & Toast

### Message Toast (Quick notification)
```javascript
sap.m.MessageToast.show("Product saved!", {
    duration: 3000,  // 3 seconds
    width: "15em"
});
```

### Message Box (Dialog)
```javascript
// Success
sap.m.MessageBox.success("Product created successfully!");

// Error
sap.m.MessageBox.error("Failed to save product");

// Warning
sap.m.MessageBox.warning("Product price is very high!");

// Information
sap.m.MessageBox.information("This is an info message");

// Confirmation
sap.m.MessageBox.confirm("Delete this product?", {
    onClose: function (oAction) {
        if (oAction === sap.m.MessageBox.Action.OK) {
            // User clicked OK
        }
    }
});
```

---

## Formatters (Data Transformation)

**Purpose:** Transform data for display

### In View:
```xml
<Text text="{
    path: 'Price',
    formatter: '.formatPrice'
}"/>

<!-- Multiple values -->
<Text text="{
    parts: ['FirstName', 'LastName'],
    formatter: '.formatFullName'
}"/>
```

### In Controller:
```javascript
formatPrice: function (sPrice) {
    if (!sPrice) {
        return "";
    }
    return "$" + parseFloat(sPrice).toFixed(2);
},

formatFullName: function (sFirstName, sLastName) {
    return sFirstName + " " + sLastName;
},

formatDate: function (oDate) {
    if (!oDate) {
        return "";
    }
    var oDateFormat = sap.ui.core.format.DateFormat.getDateInstance({
        pattern: "dd/MM/yyyy"
    });
    return oDateFormat.format(new Date(oDate));
},

formatStatus: function (sStatus) {
    var mStatusText = {
        "A": "Active",
        "I": "Inactive",
        "P": "Pending"
    };
    return mStatusText[sStatus] || "Unknown";
}
```

### Separate Formatter File:
```javascript
// util/formatter.js
sap.ui.define([], function () {
    "use strict";
    return {
        formatPrice: function (sPrice) {
            if (!sPrice) return "";
            return "$" + parseFloat(sPrice).toFixed(2);
        },
        
        formatStatus: function (sStatus) {
            var mStatusText = {
                "A": "Active",
                "I": "Inactive"
            };
            return mStatusText[sStatus] || "Unknown";
        }
    };
});
```

**Use in Controller:**
```javascript
sap.ui.define([
    "sap/ui/core/mvc/Controller",
    "com/myapp/util/formatter"
], function (Controller, formatter) {
    return Controller.extend("com.myapp.controller.Main", {
        formatter: formatter
        // Now formatters available in view as .formatter.formatPrice
    });
});
```

---

## Input Validation

### Value States:
```
"None"       → Normal (no color)
"Success"    → Green
"Warning"    → Orange
"Error"      → Red
"Information"→ Blue
```

### Example:
```javascript
onValidateEmail: function (oEvent) {
    var oInput = oEvent.getSource();
    var sValue = oInput.getValue();
    var sEmailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    
    if (!sEmailRegex.test(sValue)) {
        oInput.setValueState("Error");
        oInput.setValueStateText("Invalid email format");
    } else {
        oInput.setValueState("Success");
    }
},

onValidatePassword: function (oEvent) {
    var oInput = oEvent.getSource();
    var sValue = oInput.getValue();
    
    if (sValue.length < 8) {
        oInput.setValueState("Error");
        oInput.setValueStateText("Password must be at least 8 characters");
    } else if (sValue.length < 12) {
        oInput.setValueState("Warning");
        oInput.setValueStateText("Consider using a longer password");
    } else {
        oInput.setValueState("Success");
    }
}
```

---

## 📝 Day 5 Practice Questions

**Q21: What is a fragment?**
✅ Answer: Reusable UI component without its own controller

**Q22: How to show a message toast?**
✅ Answer: `sap.m.MessageToast.show("message")`

**Q23: What is a formatter?**
✅ Answer: Function that transforms data for display in view

**Q24: How to validate input?**
✅ Answer: Use `setValueState("Error")` and `setValueStateText("message")`

**Q25: How to load a fragment?**
✅ Answer: `Fragment.load({name, controller}).then(...)`

---

# DAY 6: Fiori Launchpad & BTP

## 🎯 Learning Goals
- Understand FLP structure
- Configure app for launchpad
- Learn BTP deployment basics
- Set up Work Zone

---

## Fiori Launchpad (FLP) Structure

```
┌──────────────────────────────────────┐
│       SAP Fiori Launchpad            │
├──────────────────────────────────────┤
│  ┌─────┐  ┌─────┐  ┌─────┐          │
│  │Tile1│  │Tile2│  │Tile3│  Group 1 │
│  └─────┘  └─────┘  └─────┘          │
│                                      │
│  ┌─────┐  ┌─────┐                   │
│  │Tile4│  │Tile5│          Group 2 │
│  └─────┘  └─────┘                   │
└──────────────────────────────────────┘

Hierarchy:
Launchpad → Catalogs → Groups → Tiles → Apps
```

---

## Configure App for FLP

### manifest.json:
```json
{
    "sap.app": {
        "crossNavigation": {
            "inbounds": {
                "intent1": {
                    "signature": {
                        "parameters": {},
                        "additionalParameters": "allowed"
                    },
                    "semanticObject": "Product",
                    "action": "display",
                    "title": "Product Management",
                    "icon": "sap-icon://product"
                }
            }
        }
    },
    "sap.flp": {
        "type": "application",
        "tileConfiguration": {
            "tileType": "sap.ushell.ui.tile.StaticTile",
            "properties": {
                "title": "Products",
                "subtitle": "Manage Products",
                "icon": "sap-icon://product",
                "targetURL": "#Product-display"
            }
        }
    }
}
```

**Remember:**
- Semantic Object: Business entity (Product, Customer, Order)
- Action: What to do (display, create, manage)
- Intent: `SemanticObject-action` (Product-display)

---

## Cross-App Navigation

### Navigate TO another app:
```javascript
var oCrossAppNav = sap.ushell.Container.getService("CrossApplicationNavigation");

oCrossAppNav.toExternal({
    target: {
        semanticObject: "Supplier",
        action: "display"
    },
    params: {
        supplierId: "S123"
    }
});
```

### Get incoming parameters:
```javascript
// In Component.js init()
var oComponentData = this.getComponentData();
if (oComponentData && oComponentData.startupParameters) {
    var sProductId = oComponentData.startupParameters.productId[0];
    // Use the parameter
}
```

---

## BTP Deployment Overview

```
┌──────────────────────────────────────┐
│  Your UI5 App (local)                │
└──────────────┬───────────────────────┘
               │
               ▼ Build MTA
┌──────────────────────────────────────┐
│  MTA Archive (.mtar file)            │
└──────────────┬───────────────────────┘
               │
               ▼ Deploy
┌──────────────────────────────────────┐
│  SAP BTP Cloud Foundry               │
│  ├─ Approuter (routing)              │
│  ├─ HTML5 Repo (app files)           │
│  ├─ XSUAA (auth)                     │
│  └─ Destination (backend)            │
└──────────────────────────────────────┘
```

---

## Key BTP Files

### 1. mta.yaml (Multi-Target Application)
```yaml
_schema-version: '3.2'
ID: myapp
version: 1.0.0

modules:
  - name: myapp-approuter
    type: approuter.nodejs
    path: approuter
    requires:
      - name: myapp-xsuaa
      - name: myapp-destination

  - name: myapp-ui
    type: html5
    path: webapp
    build-parameters:
      builder: custom
      commands:
        - npm run build

resources:
  - name: myapp-xsuaa
    type: org.cloudfoundry.managed-service
    parameters:
      service: xsuaa
      service-plan: application

  - name: myapp-destination
    type: org.cloudfoundry.managed-service
    parameters:
      service: destination
      service-plan: lite
```

### 2. xs-security.json (Authentication)
```json
{
    "xsappname": "myapp",
    "tenant-mode": "dedicated",
    "scopes": [
        {
            "name": "$XSAPPNAME.Display",
            "description": "Display products"
        },
        {
            "name": "$XSAPPNAME.Manage",
            "description": "Manage products"
        }
    ],
    "role-templates": [
        {
            "name": "Viewer",
            "scope-references": ["$XSAPPNAME.Display"]
        },
        {
            "name": "Manager",
            "scope-references": ["$XSAPPNAME.Display", "$XSAPPNAME.Manage"]
        }
    ]
}
```

### 3. xs-app.json (App Router)
```json
{
    "welcomeFile": "/index.html",
    "authenticationMethod": "route",
    "routes": [
        {
            "source": "^/sap/opu/odata/(.*)$",
            "target": "/sap/opu/odata/$1",
            "destination": "SAP_BACKEND",
            "authenticationType": "xsuaa",
            "csrfProtection": true
        },
        {
            "source": "^(.*)$",
            "target": "$1",
            "service": "html5-apps-repo-rt",
            "authenticationType": "xsuaa"
        }
    ]
}
```

---

## BTP Deployment Commands

```bash
# 1. Login to Cloud Foundry
cf login -a https://api.cf.eu10.hana.ondemand.com

# 2. Target organization and space
cf target -o myorg -s dev

# 3. Build MTA
mbt build

# 4. Deploy
cf deploy mta_archives/myapp_1.0.0.mtar

# 5. Check status
cf apps
cf services

# 6. View logs
cf logs myapp-approuter --recent

# 7. Restart app
cf restart myapp-approuter
```

---

## Work Zone Setup Steps

**Visual Flow:**
```
Deploy App → Subscribe to Work Zone → Create Site → Add Content → Assign Roles
```

### Quick Steps:
1. **Deploy** your app to BTP (cf deploy)
2. **Subscribe** to SAP Build Work Zone
3. **Assign** Launchpad_Admin role to yourself
4. **Open** Site Manager
5. **Create** a new site
6. **Add** your app from HTML5 Apps
7. **Create** Catalog and Group
8. **Create** Role and assign catalog/group
9. **Assign** role to users
10. **Launch** site!

---

## 📝 Day 6 Practice Questions

**Q26: What is semantic object in FLP?**
✅ Answer: Business entity like Product, Customer, Order

**Q27: What is an intent?**
✅ Answer: Combination of semantic object and action (Product-display)

**Q28: What is MTA?**
✅ Answer: Multi-Target Application - packaging format for BTP deployment

**Q29: What is XSUAA?**
✅ Answer: Extended Services for User Account and Authentication

**Q30: What does approuter do?**
✅ Answer: Routes requests, handles authentication, serves static content

---

# DAY 7: Review & Top 50 Questions

## 🎯 Final Day Goals
- Review all concepts
- Practice interview questions
- Prepare project examples
- Build confidence

---

## Quick Concept Review

### Day 1: Basics & MVC
✅ UI5 = HTML5 framework for enterprise apps
✅ MVC = Model (data), View (UI), Controller (logic)
✅ Component.js = App entry point
✅ Lifecycle: onInit → onBeforeRendering → onAfterRendering → onExit

### Day 2: Data Binding
✅ One-way: Model → View
✅ Two-way: Model ⇄ View
✅ Aggregation: Lists/Tables
✅ Expression: `{= ${/value} > 10 ? 'Yes' : 'No' }`

### Day 3: OData
✅ Create: `oModel.create()`
✅ Read: `oModel.read()`
✅ Update: `oModel.update()`
✅ Delete: `oModel.remove()`
✅ Filters: `new sap.ui.model.Filter()`

### Day 4: Routing
✅ Configure in manifest.json
✅ Navigate: `this.getRouter().navTo()`
✅ Parameters: `{productId}` in pattern
✅ Handle: attachPatternMatched

### Day 5: Advanced
✅ Fragments: Reusable UI
✅ Formatters: Transform data
✅ Validation: setValueState()
✅ MessageBox: Dialogs

### Day 6: FLP & BTP
✅ Intent: SemanticObject-action
✅ MTA: Deployment package
✅ XSUAA: Authentication
✅ Work Zone: Launchpad service

---

## Top 50 Interview Questions

### Basics (Q1-10)

**Q1: What is SAP UI5?**
HTML5-based JavaScript framework for building responsive enterprise web applications.

**Q2: Difference between SAP UI5 and OpenUI5?**
- SAP UI5: Commercial, more controls
- OpenUI5: Open-source, core functionality

**Q3: What is MVC?**
Model (data), View (UI), Controller (logic)

**Q4: What are view types?**
XML (most common), JSON, HTML, JavaScript, Typed

**Q5: What is Component.js?**
Main entry point that initializes app, sets up models, starts routing

**Q6: What is manifest.json?**
Application descriptor with metadata, configuration, data sources, routing

**Q7: Controller lifecycle methods?**
onInit, onBeforeRendering, onAfterRendering, onExit

**Q8: What is i18n?**
Internationalization - supporting multiple languages

**Q9: Common namespaces?**
sap.m (mobile), sap.ui.core, sap.ui.layout, sap.ui.table

**Q10: What is sap.ui.define?**
AMD-style module definition for dependency management

### Data Binding (Q11-20)

**Q11: Types of data binding?**
One-way, Two-way, One-time

**Q12: What is aggregation binding?**
Binding array data to controls like List, Table

**Q13: What is expression binding?**
Inline logic in bindings: `{= ${/price} > 100 }`

**Q14: JSONModel vs ODataModel?**
- JSONModel: Client-side, static data
- ODataModel: Server-side, REST API

**Q15: How to set model?**
```javascript
this.getView().setModel(oModel);
// Named model
this.getView().setModel(oModel, "modelName");
```

**Q16: What is element binding?**
Binding entire view/panel to single entity

**Q17: Binding modes?**
OneWay, TwoWay, OneTime

**Q18: How to get model property?**
```javascript
oModel.getProperty("/path/to/property");
```

**Q19: How to set model property?**
```javascript
oModel.setProperty("/path", value);
```

**Q20: What is context binding?**
Binding to specific data context/entity

### OData (Q21-30)

**Q21: What is OData?**
REST-based protocol for querying and updating data

**Q22: How to initialize OData model?**
```javascript
var oModel = new sap.ui.model.odata.v2.ODataModel("/service/url/");
```

**Q23: Create operation?**
```javascript
oModel.create("/EntitySet", data, {success, error});
```

**Q24: Read with filters?**
```javascript
var aFilters = [new sap.ui.model.Filter("Field", "EQ", value)];
oModel.read("/EntitySet", {filters: aFilters});
```

**Q25: What is $expand?**
Retrieve related entities in one request

**Q26: What is $select?**
Choose specific fields to retrieve

**Q27: What is batch processing?**
Combining multiple operations into single HTTP request

**Q28: Filter operators?**
EQ, NE, GT, GE, LT, LE, Contains, StartsWith

**Q29: How to handle errors?**
Use error callback in CRUD operations

**Q30: What is deep insert?**
Creating entity with related entities in one request

### Routing (Q31-40)

**Q31: How to configure routing?**
In manifest.json under sap.ui5.routing

**Q32: Route pattern with parameters?**
```json
"pattern": "product/{productId}"
```

**Q33: How to navigate?**
```javascript
this.getRouter().navTo("routeName", {params});
```

**Q34: How to get route parameters?**
```javascript
attachPatternMatched(function(oEvent) {
    var sId = oEvent.getParameter("arguments").productId;
});
```

**Q35: What are targets?**
Define which view to display for each route

**Q36: How to navigate back?**
Check history, use window.history.go(-1) or navTo home

**Q37: What is controlAggregation?**
Where router loads views (usually "pages")

**Q38: What is viewLevel?**
Transition animation level

**Q39: Master-detail pattern?**
List view + Detail view with routing

**Q40: How to bind view to entity?**
```javascript
this.getView().bindElement("/Products('123')");
```

### Advanced (Q41-50)

**Q41: What is a fragment?**
Reusable UI component without controller

**Q42: How to load fragment?**
```javascript
Fragment.load({name, controller}).then(...);
```

**Q43: What is a formatter?**
Function that transforms data for display

**Q44: How to validate input?**
```javascript
oInput.setValueState("Error");
oInput.setValueStateText("message");
```

**Q45: MessageBox vs MessageToast?**
- MessageBox: Dialog with buttons
- MessageToast: Quick notification

**Q46: What is semantic object?**
Business entity in FLP (Product, Customer)

**Q47: What is an intent?**
SemanticObject-action (Product-display)

**Q48: What is MTA?**
Multi-Target Application for BTP deployment

**Q49: What is XSUAA?**
Authentication and authorization service in BTP

**Q50: What is approuter?**
Routes requests, handles auth, serves static content

---

## 🎤 Common Interview Scenarios

### Scenario 1: Explain Your Project
**Prepare:**
```
"I worked on [App Name], a SAP Fiori app for [business purpose].
- Built using SAP UI5 with OData V2 services
- Implemented master-detail pattern
- Features: [list 2-3 key features]
- Deployed on SAP BTP Cloud Foundry
- Used [mention any specific controls or patterns]"
```

### Scenario 2: How to Debug
**Answer:**
1. Browser console (F12)
2. UI5 Diagnostics (Ctrl+Alt+Shift+S)
3. Network tab for OData calls
4. Breakpoints in code
5. Console.log for values

### Scenario 3: Performance Issues
**Answer:**
1. Use $select to fetch only needed fields
2. Enable batch requests
3. Use lazy loading (growing lists)
4. Optimize filters (server-side)
5. Destroy unused objects

### Scenario 4: Error Handling
**Answer:**
Always use success/error callbacks:
```javascript
oModel.read("/Products", {
    success: function(oData) { },
    error: function(oError) {
        MessageBox.error("Error occurred");
    }
});
```

---

## ✅ Final Checklist

Before interview:
- [ ] Review MVC concept
- [ ] Know data binding types
- [ ] Practice OData CRUD
- [ ] Understand routing
- [ ] Can explain your project
- [ ] Know FLP basics
- [ ] Understand BTP deployment
- [ ] Practice top questions
- [ ] Prepare 2-3 project examples
- [ ] Review error handling

---

## 🎯 Interview Day Tips

### Technical Round:
1. **Listen carefully** to the question
2. **Think** before answering
3. **Explain** your approach
4. **Give examples** from your experience
5. **Be honest** if you don't know

### Coding Round:
1. **Understand** requirements first
2. **Plan** your approach
3. **Write** clean, readable code
4. **Test** your code mentally
5. **Explain** what you're doing

### Behavioral Round:
1. **STAR method** (Situation, Task, Action, Result)
2. **Be specific** with examples
3. **Show learning** from challenges
4. **Be positive** about teammates
5. **Ask questions** about the role

---

## 📚 Resources for Further Learning

**Official:**
- UI5 Documentation: https://ui5.sap.com
- OpenUI5: https://openui5.org
- Demo Kit: https://ui5.sap.com/test-resources/sap/m/demokit/

**Practice:**
- Build 2-3 small apps
- Contribute to open source
- Follow SAP Community
- Join UI5 forums

---

## 🚀 You're Ready!

You've covered:
✅ UI5 Fundamentals
✅ MVC Architecture
✅ Data Binding
✅ OData Services
✅ Routing
✅ Fragments & Formatters
✅ FLP & BTP
✅ 50 Interview Questions

**Remember:**
- Practice makes perfect
- Real projects teach the most
- Stay calm and confident
- It's okay to say "I don't know, but I'd research it"

---

# Good Luck! 🎉

**You got this!** 💪
