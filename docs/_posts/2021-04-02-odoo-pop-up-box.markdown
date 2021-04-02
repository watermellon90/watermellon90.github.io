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
Btw, we still need a button from current model to open the popup form. For example, we have button in `hr.employee` model to `Upload` employee profile  

**Current model**
```
class HrEmployeeInherit(models.Model):
    _inherit = 'hr.employee'

    def action_upload_file(self):
        return {
			'name': "Upload file",
			'view_type': 'form',
			'view_mode': 'form',
			'res_model': 'popup.model',
			'view_id': self.env.ref('module_name.upload_file_form').id,
			'type': 'ir.actions.act_window',
			'target': 'new'
		}
```
**Transient model**
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
        name="action_upload_file" type="object" 
        string="Upload" 
        attrs="{'invisible': ['|', 
            ('status', '!=', 'draft'),
        ]}" />
</header>
```

**Action and Form**
```
<record id="upload_file_form" model="ir.ui.view">
    <field name="name">upload.file.form</field>
    <field name="model">popup.model</field>
    <field name="arch" type="xml">
        <form string="Upload">
            <p>Get template file</p>
            <field name="file_name" invisible="1"/>
            <field name="file_datas" file_name="file_name"/>
            <footer>
                <button name="Upload" string="Upload" class="btn-primary"/>
                <button special="cancel" string="Cancel" class="btn-secondary"/>
            </footer>
        </form>
    </field>
</record>

<record id="upload_file_action" model="ir.actions.act_window">
    <field name="name">Upload files</field>
    <field name="res_model">popup.model</field>
    <field name="view_type">form</field>
    <field name="view_mode">form</field>
    <field name="target">new</field>
</record>
```