# Ember-attachable

[`ember-cli`](https://github.com/stefanpenner/ember-cli) package, which adds attachments support to your Ember-Data models.

## Installing

In the root dir of your project run

`npm i ember-attachable --save-dev`,

*or* you can add `ember-attachable` to your `package.json`:

```javascript
"devDependencies": {
  ...
  "ember-attachable": "latest"
}
```
You may want to be more precise with your version locking.

## Usage

`Ember-attachable` provides a mixin to be included in your models for
adding attachments support. This mixin can be imported from
your app's namespace (e.g. `../mixins/attachable` in your models):

```javascript
import Ember from 'ember';
import DS from 'ember-data';

import Attachable from '../mixins/attachable';

export default DS.Model.extend(Attachable, {
  attachmentAs: 'file'; // Name of your attachable attribute
});
```
There is a support of having *two or more* attachments simultaneously
on the same model. For this, just set _array_ of strings as a value of
`attachmentAs` property:

```javascript
export default DS.Model.extend(Attachable, {
  attachmentAs: ['file', 'photo']; // Name of your attachable attributes
});
```

To save your model with attachment(s), mixin adds a new method `saveWithAttachement()`.
This method _adheres_ `Ember-Data`'s `save()` semantics, and saves your model
along with attachment:

```javascript
userModel.set('photo', file) // 'photo' is the name of attachment configured in userModel's class
userModel.saveWithAttachment()
```
Attachment itself can be an instance of [`Blob`] (https://developer.mozilla.org/en-US/docs/Web/API/Blob)
or of any other classes which are supported by `FormData` (see [`Working principle`](#working-principle) below)

## Working principle

`ember-attachable` internally uses [`FormData API`](https://developer.mozilla.org/en-US/docs/Web/API/FormData)
to build _POST_ request with
`Content-Type: multipart/form-data` for saving your `Ember Data` models along with transferring attachment.
If you ember app is backed by `Rails` application, you can use this library with [`paperclip gem`](https://github.com/thoughtbot/paperclip)
(or any other of your taste) to effectively manage _*save* request_ on backend.
