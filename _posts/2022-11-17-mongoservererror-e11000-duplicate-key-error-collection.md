

        ---
        title: MongoServerError: E11000 duplicate key error collection
        date: 2022-11-17 13:17:10.773 +0000
        categories: [error]
        tags: ['mongodb', 'nodejs', '에러']
        description: 노마드코더 유튜브 클론 강의를 듣던 중, Model.create()를 통한 유저 생성 기능에서 오류가 발생했다.github_1라는 유니크 인덱스(일종의 키값)를 사용하는 유저가 이미 있어서, 이로인해 중복이 발생했다는 메세지이다.분명히 처음엔 잘 작동했었는데, gith
        
        
        ---

        ## 에러 발생
노마드코더 유튜브 클론 강의를 듣던 중, `Model.create()`를 통한 유저 생성 기능에서 오류가 발생했다.
```
MongoServerError: E11000 duplicate key error collection: ...users index: github_1 dup key: { github: null }
```
github_1라는 유니크 인덱스(일종의 키값)를 사용하는 유저가 이미 있어서, 이로인해 중복이 발생했다는 메세지이다.

분명히 처음엔 잘 작동했었는데, github 연동 로그인 기능을 추가하면서 modelSchema를 수정한 것이 오류의 발생 원인으로 보인다.

테스트를 위해 유저를 생성하고, 제거하는 과정에서 이전 model의 collection이 남아있던 것이 원인이 아닐까 싶다.

## 에러 해결
### `db.collections.getIndexes()`
먼저 mongoDB에서 충돌이 일어나는 collections의 인덱스 목록을 가져온다.
그러면 중복이 발생한 인덱스를 찾을 수 있을 것이다.

### `db.collections.dropIndex({filter});`
해당 인덱스를 지정하여 제거하면 된다.
나의 경우는 `db.users.dropIndex({"github": 1})`로 제거하였다.

        