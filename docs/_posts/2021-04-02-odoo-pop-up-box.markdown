---
layout: post
title:  "Backend pop up box"
date:   2021-04-02 14:00:37 +0700
categories: odoo backend
---

## Scenerio 1: Button to pop-up
Start with click to button in the left of state bar and it will be show the popup with content

### **Model**  
Define a `TransientModel` to hold all information *in form*  
```
class PopupModel(models.TransientModel):
    _name = "popup.model"
    _description = "For example of scenerio 1"

    file_name = fields.Char(string="File name")
    file_datas = fields.Binary(string="Datas")
```

### **View**  
**Button in header**
```
<header>
    <button 
        name="Upload" type="object" 
        string="Download key" 
        attrs="{'invisible': ['|', 
            ('status', '!=', 'draft'),
        ]}" />
</header>
```