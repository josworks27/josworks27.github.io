---
layout: post
title: "Sequelize Hooks"
tags: 
    - Immersive 16
comments: true
---


# Hooks
Hooks는 Sequelize가 실행되기 전후에 호출되는 함수이다. 예를 들어 항상 모델에 저장하기 전에 값을 설정하고 싶다면 beforeUpdate hook를 사용하면 된다.

> Note: 인스턴스에는 Hook를 사용할 수 없다. 오직 모델에서 사용된다.

**Hooks 오퍼레이션의 작동순서**
```javascript
(1)
  beforeBulkCreate(instances, options)
  beforeBulkDestroy(options)
  beforeBulkUpdate(options)
(2)
  beforeValidate(instance, options)
(-)
  validate
(3)
  afterValidate(instance, options)
  - or -
  validationFailed(instance, options, error)
(4)
  beforeCreate(instance, options)
  beforeDestroy(instance, options)
  beforeUpdate(instance, options)
  beforeSave(instance, options)
  beforeUpsert(values, options)
(-)
  create
  destroy
  update
(5)
  afterCreate(instance, options)
  afterDestroy(instance, options)
  afterUpdate(instance, options)
  afterSave(instance, options)
  afterUpsert(created, options)
(6)
  afterBulkCreate(instances, options)
  afterBulkDestroy(options)
  afterBulkUpdate(options)
```

**Hooks를 이용한 패스워드 암호화 방법**

```javascript
afterValidate : (data, options) => {
    let salt = 'random string';
    let shasum = ctypto.createHash('sha1')
    shasum.update(data.url + salt);
    data.code = shasum.digest('hex');
}
```