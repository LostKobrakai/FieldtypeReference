# Fieldtype Reference

This fieldtype references a field in a page, where the field's data is retrieved from a somehow related remote page. This is done in two steps. Each field needs a PHP snippet, which will return the remote page, that holds the field, that you want to provide a reference to. The second step is providing pairs of templates and fields. There you can set which field will be referenced, depending on which template the selected page is of. It's kinda like a one way symlink to a remote field.

## Example

Custom PHP Code:
```php
return $page->parents("template=rootOfSubtree|intermediateTemplate")->first();
```

Template / Field Pairing
```
rootOfSubtree=title
intermediateTemplate=checkbox
```

This will add an uneditable field to your admin backend, which will show the `title`- or `checkbox`-value of the selected page. The backend field can be hidden with collapsed modes, if needed. You can access the field from the api, too, but also in read-only mode, so setting data to it won't work.

## Use Cases

### Inputfield Dependencies

Inputfield dependencies can use this as a shortcut. E.g. you want to show a field only if a checkbox in the homepage is checked.
```php
return $pages->get("/");
```
```
home=checkbox
```
```markdown
// show-if selector
// works on any page, wherever in the tree it might be
reference=1
```

### Page Fields using the "Selector Value" option

Page fields can be dynamically limited by fields of the same page. Imaging a page field, which lets you select a list of shoes. Now we can limit the selectable shoes by any field related to the page the shoe pagefield is on.
```
template=shoes, brand=reference
```

`reference` is in this case a link to a field, which returns a brand-`Page`-object.