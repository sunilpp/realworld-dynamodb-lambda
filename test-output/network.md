```
DELETE /__TESTUTILS__/purge
```
```
200 OK

"Purged all data!"
```
# Article
```
POST /users

{
  "user": {
    "email": "author-9ur06u@email.com",
    "username": "author-9ur06u",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "author-9ur06u@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImF1dGhvci05dXIwNnUiLCJpYXQiOjE1Nzg4Njc5ODQsImV4cCI6MTU3OTA0MDc4NH0.NtsrtKr0xC_HKrpIVQvUd_KmsKi9qW0f34UVcXzZlYE",
    "username": "author-9ur06u",
    "bio": "",
    "image": ""
  }
}
```
```
POST /users

{
  "user": {
    "email": "authoress-rvsmvw@email.com",
    "username": "authoress-rvsmvw",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "authoress-rvsmvw@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImF1dGhvcmVzcy1ydnNtdnciLCJpYXQiOjE1Nzg4Njc5ODQsImV4cCI6MTU3OTA0MDc4NH0.1Ii76_WhN6yFLDmLORksH2W-IVwkcv2cy8pAp_CyiJI",
    "username": "authoress-rvsmvw",
    "bio": "",
    "image": ""
  }
}
```
```
POST /users

{
  "user": {
    "email": "non-author-hg0wlk@email.com",
    "username": "non-author-hg0wlk",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "non-author-hg0wlk@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Im5vbi1hdXRob3ItaGcwd2xrIiwiaWF0IjoxNTc4ODY3OTg0LCJleHAiOjE1NzkwNDA3ODR9.wCG9Em3GqptmeypjzKJRqGvmiXSWxTIFwX9ar_aVmqU",
    "username": "non-author-hg0wlk",
    "bio": "",
    "image": ""
  }
}
```
## Create
### should create article
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body"
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-iqd2uw",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867984244,
    "updatedAt": 1578867984244,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
### should create article with tags
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "tag_a",
      "tag_b"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-kfan1g",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867984311,
    "updatedAt": 1578867984311,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
### should disallow unauthenticated user
```
POST /articles

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should enforce required fields
```
POST /articles

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article must be specified."
    ]
  }
}
```
```
POST /articles

{
  "article": {}
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "title must be specified."
    ]
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "description must be specified."
    ]
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "body must be specified."
    ]
  }
}
```
## Get
### should get article by slug
```
GET /articles/title-iqd2uw
```
```
200 OK

{
  "article": {
    "createdAt": 1578867984244,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "slug": "title-iqd2uw",
    "updatedAt": 1578867984244,
    "tagList": [],
    "favoritesCount": 0,
    "favorited": false
  }
}
```
### should get article with tags by slug
```
GET /articles/title-kfan1g
```
```
200 OK

{
  "article": {
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "createdAt": 1578867984311,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "slug": "title-kfan1g",
    "updatedAt": 1578867984311,
    "favoritesCount": 0,
    "favorited": false
  }
}
```
### should disallow unknown slug
```
GET /articles/uh84a
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [uh84a]"
    ]
  }
}
```
## Update
### should update article
```
PUT /articles/title-kfan1g

{
  "article": {
    "title": "newtitle"
  }
}
```
```
200 OK

{
  "article": {
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "createdAt": 1578867984311,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "newtitle",
    "body": "body",
    "slug": "title-kfan1g",
    "updatedAt": 1578867984311,
    "favoritesCount": 0,
    "favorited": false
  }
}
```
```
PUT /articles/title-kfan1g

{
  "article": {
    "description": "newdescription"
  }
}
```
```
200 OK

{
  "article": {
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "createdAt": 1578867984311,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "newdescription",
    "title": "newtitle",
    "body": "body",
    "slug": "title-kfan1g",
    "updatedAt": 1578867984311,
    "favoritesCount": 0,
    "favorited": false
  }
}
```
```
PUT /articles/title-kfan1g

{
  "article": {
    "body": "newbody"
  }
}
```
```
200 OK

{
  "article": {
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "createdAt": 1578867984311,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "newdescription",
    "title": "newtitle",
    "body": "newbody",
    "slug": "title-kfan1g",
    "updatedAt": 1578867984311,
    "favoritesCount": 0,
    "favorited": false
  }
}
```
### should disallow missing mutation
```
PUT /articles/title-kfan1g

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article mutation must be specified."
    ]
  }
}
```
### should disallow empty mutation
```
PUT /articles/title-kfan1g

{
  "article": {}
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "At least one field must be specified: [title, description, article]."
    ]
  }
}
```
### should disallow unauthenticated update
```
PUT /articles/title-kfan1g

{
  "article": {
    "title": "newtitle"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should disallow updating non-existent article
```
PUT /articles/foo-title-kfan1g

{
  "article": {
    "title": "newtitle"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [foo-title-kfan1g]"
    ]
  }
}
```
### should disallow non-author from updating
```
PUT /articles/title-kfan1g

{
  "article": {
    "title": "newtitle"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article can only be updated by author: [author-9ur06u]"
    ]
  }
}
```
## Favorite
### should favorite article
```
POST /articles/title-iqd2uw/favorite

{}
```
```
200 OK

{
  "article": {
    "createdAt": 1578867984244,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "slug": "title-iqd2uw",
    "updatedAt": 1578867984244,
    "favoritedBy": [
      "non-author-hg0wlk"
    ],
    "favoritesCount": 1,
    "tagList": [],
    "favorited": true
  }
}
```
```
GET /articles/title-iqd2uw
```
```
200 OK

{
  "article": {
    "createdAt": 1578867984244,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "favoritesCount": 1,
    "slug": "title-iqd2uw",
    "updatedAt": 1578867984244,
    "tagList": [],
    "favorited": true
  }
}
```
### should disallow favoriting by unauthenticated user
```
POST /articles/title-iqd2uw/favorite

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should disallow favoriting unknown article
```
POST /articles/title-iqd2uw_foo/favorite

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [title-iqd2uw_foo]"
    ]
  }
}
```
### should unfavorite article
```
DELETE /articles/title-iqd2uw/favorite
```
```
200 OK

{
  "article": {
    "createdAt": 1578867984244,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "favoritesCount": 0,
    "slug": "title-iqd2uw",
    "updatedAt": 1578867984244,
    "tagList": [],
    "favorited": false
  }
}
```
## Delete
### should delete article
```
DELETE /articles/title-iqd2uw
```
```
200 OK

{}
```
```
GET /articles/title-iqd2uw
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [title-iqd2uw]"
    ]
  }
}
```
### should disallow deleting by unauthenticated user
```
DELETE /articles/foo
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should disallow deleting unknown article
```
DELETE /articles/foobar
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [foobar]"
    ]
  }
}
```
### should disallow deleting article by non-author
```
DELETE /articles/title-kfan1g
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article can only be deleted by author: [author-9ur06u]"
    ]
  }
}
```
## List
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "lcgc7",
      "tag_0",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-rkh1qc",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985164,
    "updatedAt": 1578867985164,
    "author": {
      "username": "authoress-rvsmvw",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "lcgc7",
      "tag_0",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "9guec2",
      "tag_1",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-1qkqdl",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985203,
    "updatedAt": 1578867985203,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "9guec2",
      "tag_1",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "rvlc2w",
      "tag_2",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-zaoxja",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985226,
    "updatedAt": 1578867985226,
    "author": {
      "username": "authoress-rvsmvw",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "rvlc2w",
      "tag_2",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "j5l1sn",
      "tag_3",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-2rslm0",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985250,
    "updatedAt": 1578867985250,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "j5l1sn",
      "tag_3",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "7t20q0",
      "tag_4",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-pygf26",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985269,
    "updatedAt": 1578867985269,
    "author": {
      "username": "authoress-rvsmvw",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "7t20q0",
      "tag_4",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "2dxqr5",
      "tag_5",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-iissov",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985289,
    "updatedAt": 1578867985289,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "2dxqr5",
      "tag_5",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "58n1ji",
      "tag_6",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-xnx7v8",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985310,
    "updatedAt": 1578867985310,
    "author": {
      "username": "authoress-rvsmvw",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "58n1ji",
      "tag_6",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "7e2cwz",
      "tag_7",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-24gj93",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985331,
    "updatedAt": 1578867985331,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "7e2cwz",
      "tag_7",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "3quw4u",
      "tag_8",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-lmnwjo",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985353,
    "updatedAt": 1578867985353,
    "author": {
      "username": "authoress-rvsmvw",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "3quw4u",
      "tag_8",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "ebohud",
      "tag_9",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-t3ppd1",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985382,
    "updatedAt": 1578867985382,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "ebohud",
      "tag_9",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "mwcscu",
      "tag_10",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-5skxit",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985404,
    "updatedAt": 1578867985404,
    "author": {
      "username": "authoress-rvsmvw",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "mwcscu",
      "tag_10",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "jy026j",
      "tag_11",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-hgzjmh",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985426,
    "updatedAt": 1578867985426,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "jy026j",
      "tag_11",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "l2gl7j",
      "tag_12",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-3gvw7t",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985448,
    "updatedAt": 1578867985448,
    "author": {
      "username": "authoress-rvsmvw",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "l2gl7j",
      "tag_12",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "u5463t",
      "tag_13",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-uxs6xi",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985469,
    "updatedAt": 1578867985469,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "u5463t",
      "tag_13",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "emrnmj",
      "tag_14",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-tqwfpe",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985495,
    "updatedAt": 1578867985495,
    "author": {
      "username": "authoress-rvsmvw",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "emrnmj",
      "tag_14",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "5d1haj",
      "tag_15",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-3sb3ay",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985515,
    "updatedAt": 1578867985515,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "5d1haj",
      "tag_15",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "t8xm7d",
      "tag_16",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-8eehqh",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985536,
    "updatedAt": 1578867985536,
    "author": {
      "username": "authoress-rvsmvw",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "t8xm7d",
      "tag_16",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "guck8o",
      "tag_17",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-7e0c4l",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985562,
    "updatedAt": 1578867985562,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "guck8o",
      "tag_17",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "menwjy",
      "tag_18",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-usy6oo",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985586,
    "updatedAt": 1578867985586,
    "author": {
      "username": "authoress-rvsmvw",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "menwjy",
      "tag_18",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "uu4fc7",
      "tag_19",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-z9tuh6",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867985617,
    "updatedAt": 1578867985617,
    "author": {
      "username": "author-9ur06u",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "uu4fc7",
      "tag_19",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
### should list articles
```
GET /articles
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "tag_19",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "uu4fc7"
      ],
      "createdAt": 1578867985617,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-z9tuh6",
      "updatedAt": 1578867985617,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "menwjy",
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985586,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-usy6oo",
      "updatedAt": 1578867985586,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "guck8o",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985562,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-7e0c4l",
      "updatedAt": 1578867985562,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "t8xm7d",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985536,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-8eehqh",
      "updatedAt": 1578867985536,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "5d1haj",
        "tag_15",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985515,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3sb3ay",
      "updatedAt": 1578867985515,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "emrnmj",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985495,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-tqwfpe",
      "updatedAt": 1578867985495,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_13",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "u5463t"
      ],
      "createdAt": 1578867985469,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-uxs6xi",
      "updatedAt": 1578867985469,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "l2gl7j",
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985448,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3gvw7t",
      "updatedAt": 1578867985448,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "jy026j",
        "tag_11",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985426,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-hgzjmh",
      "updatedAt": 1578867985426,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "mwcscu",
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985404,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-5skxit",
      "updatedAt": 1578867985404,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "ebohud",
        "tag_9",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985382,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-t3ppd1",
      "updatedAt": 1578867985382,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "3quw4u",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985353,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-lmnwjo",
      "updatedAt": 1578867985353,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "7e2cwz",
        "tag_7",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985331,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-24gj93",
      "updatedAt": 1578867985331,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "58n1ji",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985310,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-xnx7v8",
      "updatedAt": 1578867985310,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "2dxqr5",
        "tag_5",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985289,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-iissov",
      "updatedAt": 1578867985289,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "7t20q0",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985269,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-pygf26",
      "updatedAt": 1578867985269,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "j5l1sn",
        "tag_3",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985250,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-2rslm0",
      "updatedAt": 1578867985250,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "rvlc2w",
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985226,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zaoxja",
      "updatedAt": 1578867985226,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "9guec2",
        "tag_1",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985203,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-1qkqdl",
      "updatedAt": 1578867985203,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "lcgc7",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985164,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-rkh1qc",
      "updatedAt": 1578867985164,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should list articles with tag
```
GET /articles?tag=tag_7
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "7e2cwz",
        "tag_7",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985331,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-24gj93",
      "updatedAt": 1578867985331,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
```
GET /articles?tag=tag_mod_3_2
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "guck8o",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985562,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-7e0c4l",
      "updatedAt": 1578867985562,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "emrnmj",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985495,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-tqwfpe",
      "updatedAt": 1578867985495,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "jy026j",
        "tag_11",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985426,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-hgzjmh",
      "updatedAt": 1578867985426,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "3quw4u",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985353,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-lmnwjo",
      "updatedAt": 1578867985353,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "2dxqr5",
        "tag_5",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985289,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-iissov",
      "updatedAt": 1578867985289,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "rvlc2w",
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985226,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zaoxja",
      "updatedAt": 1578867985226,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should list articles by author
```
GET /articles?author=authoress-rvsmvw
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "menwjy",
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985586,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-usy6oo",
      "updatedAt": 1578867985586,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "t8xm7d",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985536,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-8eehqh",
      "updatedAt": 1578867985536,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "emrnmj",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985495,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-tqwfpe",
      "updatedAt": 1578867985495,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "l2gl7j",
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985448,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3gvw7t",
      "updatedAt": 1578867985448,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "mwcscu",
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985404,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-5skxit",
      "updatedAt": 1578867985404,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "3quw4u",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985353,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-lmnwjo",
      "updatedAt": 1578867985353,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "58n1ji",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985310,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-xnx7v8",
      "updatedAt": 1578867985310,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "7t20q0",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985269,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-pygf26",
      "updatedAt": 1578867985269,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "rvlc2w",
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985226,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zaoxja",
      "updatedAt": 1578867985226,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "lcgc7",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985164,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-rkh1qc",
      "updatedAt": 1578867985164,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should list articles favorited by user
```
GET /articles?favorited=non-author-hg0wlk
```
```
200 OK

{
  "articles": []
}
```
### should list articles by limit/offset
```
GET /articles?author=author-9ur06u&limit=2
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "tag_19",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "uu4fc7"
      ],
      "createdAt": 1578867985617,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-z9tuh6",
      "updatedAt": 1578867985617,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "guck8o",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985562,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-7e0c4l",
      "updatedAt": 1578867985562,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
```
GET /articles?author=author-9ur06u&limit=2&offset=2
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "5d1haj",
        "tag_15",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985515,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3sb3ay",
      "updatedAt": 1578867985515,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_13",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "u5463t"
      ],
      "createdAt": 1578867985469,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-uxs6xi",
      "updatedAt": 1578867985469,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should list articles when authenticated
```
GET /articles
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "tag_19",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "uu4fc7"
      ],
      "createdAt": 1578867985617,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-z9tuh6",
      "updatedAt": 1578867985617,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "menwjy",
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985586,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-usy6oo",
      "updatedAt": 1578867985586,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "guck8o",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985562,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-7e0c4l",
      "updatedAt": 1578867985562,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "t8xm7d",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985536,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-8eehqh",
      "updatedAt": 1578867985536,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "5d1haj",
        "tag_15",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985515,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3sb3ay",
      "updatedAt": 1578867985515,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "emrnmj",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985495,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-tqwfpe",
      "updatedAt": 1578867985495,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_13",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "u5463t"
      ],
      "createdAt": 1578867985469,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-uxs6xi",
      "updatedAt": 1578867985469,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "l2gl7j",
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985448,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3gvw7t",
      "updatedAt": 1578867985448,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "jy026j",
        "tag_11",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985426,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-hgzjmh",
      "updatedAt": 1578867985426,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "mwcscu",
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985404,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-5skxit",
      "updatedAt": 1578867985404,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "ebohud",
        "tag_9",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985382,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-t3ppd1",
      "updatedAt": 1578867985382,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "3quw4u",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985353,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-lmnwjo",
      "updatedAt": 1578867985353,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "7e2cwz",
        "tag_7",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985331,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-24gj93",
      "updatedAt": 1578867985331,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "58n1ji",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985310,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-xnx7v8",
      "updatedAt": 1578867985310,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "2dxqr5",
        "tag_5",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985289,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-iissov",
      "updatedAt": 1578867985289,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "7t20q0",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985269,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-pygf26",
      "updatedAt": 1578867985269,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "j5l1sn",
        "tag_3",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985250,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-2rslm0",
      "updatedAt": 1578867985250,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "rvlc2w",
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985226,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zaoxja",
      "updatedAt": 1578867985226,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "9guec2",
        "tag_1",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985203,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-1qkqdl",
      "updatedAt": 1578867985203,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "lcgc7",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985164,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-rkh1qc",
      "updatedAt": 1578867985164,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should disallow multiple of author/tag/favorited
```
GET /articles?tag=foo&author=bar
```
```
GET /articles?author=foo&favorited=bar
```
```
GET /articles?favorited=foo&tag=bar
```
## Feed
### should get feed
```
GET /articles/feed
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Only one of these can be specified: [tag, author, favorited]"
    ]
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Only one of these can be specified: [tag, author, favorited]"
    ]
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Only one of these can be specified: [tag, author, favorited]"
    ]
  }
}
```
```
200 OK

{
  "articles": []
}
```
```
POST /profiles/authoress-rvsmvw/follow

{}
```
```
200 OK

{
  "profile": {
    "username": "authoress-rvsmvw",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
GET /articles/feed
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "menwjy",
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985586,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-usy6oo",
      "updatedAt": 1578867985586,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "t8xm7d",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985536,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-8eehqh",
      "updatedAt": 1578867985536,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "emrnmj",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985495,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-tqwfpe",
      "updatedAt": 1578867985495,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "l2gl7j",
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985448,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3gvw7t",
      "updatedAt": 1578867985448,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "mwcscu",
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985404,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-5skxit",
      "updatedAt": 1578867985404,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "3quw4u",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985353,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-lmnwjo",
      "updatedAt": 1578867985353,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "58n1ji",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985310,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-xnx7v8",
      "updatedAt": 1578867985310,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "7t20q0",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985269,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-pygf26",
      "updatedAt": 1578867985269,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "rvlc2w",
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985226,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zaoxja",
      "updatedAt": 1578867985226,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "lcgc7",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985164,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-rkh1qc",
      "updatedAt": 1578867985164,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
```
POST /profiles/author-9ur06u/follow

{}
```
```
200 OK

{
  "profile": {
    "username": "author-9ur06u",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
GET /articles/feed
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "tag_19",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "uu4fc7"
      ],
      "createdAt": 1578867985617,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-z9tuh6",
      "updatedAt": 1578867985617,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "menwjy",
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985586,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-usy6oo",
      "updatedAt": 1578867985586,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "guck8o",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985562,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-7e0c4l",
      "updatedAt": 1578867985562,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "t8xm7d",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985536,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-8eehqh",
      "updatedAt": 1578867985536,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "5d1haj",
        "tag_15",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985515,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3sb3ay",
      "updatedAt": 1578867985515,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "emrnmj",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985495,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-tqwfpe",
      "updatedAt": 1578867985495,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_13",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "u5463t"
      ],
      "createdAt": 1578867985469,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-uxs6xi",
      "updatedAt": 1578867985469,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "l2gl7j",
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985448,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3gvw7t",
      "updatedAt": 1578867985448,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "jy026j",
        "tag_11",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985426,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-hgzjmh",
      "updatedAt": 1578867985426,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "mwcscu",
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985404,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-5skxit",
      "updatedAt": 1578867985404,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "ebohud",
        "tag_9",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985382,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-t3ppd1",
      "updatedAt": 1578867985382,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "3quw4u",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985353,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-lmnwjo",
      "updatedAt": 1578867985353,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "7e2cwz",
        "tag_7",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985331,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-24gj93",
      "updatedAt": 1578867985331,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "58n1ji",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985310,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-xnx7v8",
      "updatedAt": 1578867985310,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "2dxqr5",
        "tag_5",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985289,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-iissov",
      "updatedAt": 1578867985289,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "7t20q0",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985269,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-pygf26",
      "updatedAt": 1578867985269,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "j5l1sn",
        "tag_3",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985250,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-2rslm0",
      "updatedAt": 1578867985250,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "rvlc2w",
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1578867985226,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zaoxja",
      "updatedAt": 1578867985226,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "9guec2",
        "tag_1",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1578867985203,
      "author": {
        "username": "author-9ur06u",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-1qkqdl",
      "updatedAt": 1578867985203,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "lcgc7",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1578867985164,
      "author": {
        "username": "authoress-rvsmvw",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-rkh1qc",
      "updatedAt": 1578867985164,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should disallow unauthenticated feed
```
GET /articles/feed
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
## Tags
### should get tags
```
GET /tags
```
```
200 OK

{
  "tags": [
    "emrnmj",
    "tag_14",
    "tag_mod_2_0",
    "tag_mod_3_2",
    "jy026j",
    "tag_11",
    "tag_mod_2_1",
    "ebohud",
    "tag_9",
    "tag_mod_3_0",
    "guck8o",
    "tag_17",
    "tag_13",
    "tag_mod_3_1",
    "u5463t",
    "lcgc7",
    "tag_0",
    "t8xm7d",
    "tag_16",
    "tag_a",
    "tag_b",
    "2dxqr5",
    "tag_5",
    "3quw4u",
    "tag_8",
    "rvlc2w",
    "tag_2",
    "tag_19",
    "uu4fc7",
    "j5l1sn",
    "tag_3",
    "l2gl7j",
    "tag_12",
    "7t20q0",
    "tag_4",
    "menwjy",
    "tag_18",
    "mwcscu",
    "tag_10",
    "9guec2",
    "tag_1",
    "58n1ji",
    "tag_6",
    "7e2cwz",
    "tag_7",
    "5d1haj",
    "tag_15"
  ]
}
```
# Comment
```
POST /users

{
  "user": {
    "email": "author-niy06h@email.com",
    "username": "author-niy06h",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "author-niy06h@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImF1dGhvci1uaXkwNmgiLCJpYXQiOjE1Nzg4Njc5ODYsImV4cCI6MTU3OTA0MDc4Nn0.p-URkT3unFukvHg9n_tEuDhimY8AampE2vSG6qqX-Kc",
    "username": "author-niy06h",
    "bio": "",
    "image": ""
  }
}
```
```
POST /users

{
  "user": {
    "email": "commenter-vuctdu@email.com",
    "username": "commenter-vuctdu",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "commenter-vuctdu@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImNvbW1lbnRlci12dWN0ZHUiLCJpYXQiOjE1Nzg4Njc5ODYsImV4cCI6MTU3OTA0MDc4Nn0.9NXc3P5q-xeM4IWWhl42RVW0jl1-6Cthc9XUsfWgvQg",
    "username": "commenter-vuctdu",
    "bio": "",
    "image": ""
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body"
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-tpbrqo",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1578867986848,
    "updatedAt": 1578867986848,
    "author": {
      "username": "author-niy06h",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
## Create
### should create comment
```
POST /articles/title-tpbrqo/comments

{
  "comment": {
    "body": "test comment dcigsu"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "e5f9e267-fd83-4d56-a265-f17b8ce8d4dc",
    "slug": "title-tpbrqo",
    "body": "test comment dcigsu",
    "createdAt": 1578867986870,
    "updatedAt": 1578867986870,
    "author": {
      "username": "commenter-vuctdu",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-tpbrqo/comments

{
  "comment": {
    "body": "test comment rufgkp"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "0fca98eb-ecfe-42bb-a294-4cbc108cd0d2",
    "slug": "title-tpbrqo",
    "body": "test comment rufgkp",
    "createdAt": 1578867986910,
    "updatedAt": 1578867986910,
    "author": {
      "username": "commenter-vuctdu",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-tpbrqo/comments

{
  "comment": {
    "body": "test comment t9nglx"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "c2bc6609-d171-4955-a006-405c47f2fc4e",
    "slug": "title-tpbrqo",
    "body": "test comment t9nglx",
    "createdAt": 1578867986938,
    "updatedAt": 1578867986938,
    "author": {
      "username": "commenter-vuctdu",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-tpbrqo/comments

{
  "comment": {
    "body": "test comment 2a9eg4"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "80f62212-54cb-44c6-a472-43291611f5db",
    "slug": "title-tpbrqo",
    "body": "test comment 2a9eg4",
    "createdAt": 1578867986963,
    "updatedAt": 1578867986963,
    "author": {
      "username": "commenter-vuctdu",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-tpbrqo/comments

{
  "comment": {
    "body": "test comment j3is0x"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "cccbe594-ef92-4bfa-bfa5-47d2f1ab0f9b",
    "slug": "title-tpbrqo",
    "body": "test comment j3is0x",
    "createdAt": 1578867987012,
    "updatedAt": 1578867987012,
    "author": {
      "username": "commenter-vuctdu",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-tpbrqo/comments

{
  "comment": {
    "body": "test comment nameoe"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "e6bd0ce2-cf28-465c-9f90-79ea56e4b43c",
    "slug": "title-tpbrqo",
    "body": "test comment nameoe",
    "createdAt": 1578867987038,
    "updatedAt": 1578867987038,
    "author": {
      "username": "commenter-vuctdu",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-tpbrqo/comments

{
  "comment": {
    "body": "test comment mdwiyk"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "a4fbe578-6081-492d-aabf-3d1f3cebea1a",
    "slug": "title-tpbrqo",
    "body": "test comment mdwiyk",
    "createdAt": 1578867987064,
    "updatedAt": 1578867987064,
    "author": {
      "username": "commenter-vuctdu",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-tpbrqo/comments

{
  "comment": {
    "body": "test comment 1uc80h"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "d7d31f50-01ad-4697-bc31-b7d36c3fcee2",
    "slug": "title-tpbrqo",
    "body": "test comment 1uc80h",
    "createdAt": 1578867987085,
    "updatedAt": 1578867987085,
    "author": {
      "username": "commenter-vuctdu",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-tpbrqo/comments

{
  "comment": {
    "body": "test comment rfapf9"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "e1c96767-a003-48dc-a6f3-52ab3f215d0c",
    "slug": "title-tpbrqo",
    "body": "test comment rfapf9",
    "createdAt": 1578867987115,
    "updatedAt": 1578867987115,
    "author": {
      "username": "commenter-vuctdu",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-tpbrqo/comments

{
  "comment": {
    "body": "test comment rzq88d"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "9141f308-2b47-4cd9-980f-578249e1b2f3",
    "slug": "title-tpbrqo",
    "body": "test comment rzq88d",
    "createdAt": 1578867987139,
    "updatedAt": 1578867987139,
    "author": {
      "username": "commenter-vuctdu",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
### should disallow unauthenticated user
```
POST /articles/title-tpbrqo/comments

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should enforce comment body
```
POST /articles/title-tpbrqo/comments

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Comment must be specified."
    ]
  }
}
```
### should disallow non-existent article
```
POST /articles/foobar/comments

{
  "comment": {
    "body": "test comment vygpg2"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [foobar]"
    ]
  }
}
```
## Get
### should get all comments for article
```
GET /articles/title-tpbrqo/comments
```
```
200 OK

{
  "comments": [
    {
      "createdAt": 1578867987012,
      "id": "cccbe594-ef92-4bfa-bfa5-47d2f1ab0f9b",
      "body": "test comment j3is0x",
      "slug": "title-tpbrqo",
      "author": {
        "username": "commenter-vuctdu",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1578867987012
    },
    {
      "createdAt": 1578867986910,
      "id": "0fca98eb-ecfe-42bb-a294-4cbc108cd0d2",
      "body": "test comment rufgkp",
      "slug": "title-tpbrqo",
      "author": {
        "username": "commenter-vuctdu",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1578867986910
    },
    {
      "createdAt": 1578867986870,
      "id": "e5f9e267-fd83-4d56-a265-f17b8ce8d4dc",
      "body": "test comment dcigsu",
      "slug": "title-tpbrqo",
      "author": {
        "username": "commenter-vuctdu",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1578867986870
    },
    {
      "createdAt": 1578867987115,
      "id": "e1c96767-a003-48dc-a6f3-52ab3f215d0c",
      "body": "test comment rfapf9",
      "slug": "title-tpbrqo",
      "author": {
        "username": "commenter-vuctdu",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1578867987115
    },
    {
      "createdAt": 1578867986938,
      "id": "c2bc6609-d171-4955-a006-405c47f2fc4e",
      "body": "test comment t9nglx",
      "slug": "title-tpbrqo",
      "author": {
        "username": "commenter-vuctdu",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1578867986938
    },
    {
      "createdAt": 1578867986963,
      "id": "80f62212-54cb-44c6-a472-43291611f5db",
      "body": "test comment 2a9eg4",
      "slug": "title-tpbrqo",
      "author": {
        "username": "commenter-vuctdu",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1578867986963
    },
    {
      "createdAt": 1578867987139,
      "id": "9141f308-2b47-4cd9-980f-578249e1b2f3",
      "body": "test comment rzq88d",
      "slug": "title-tpbrqo",
      "author": {
        "username": "commenter-vuctdu",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1578867987139
    },
    {
      "createdAt": 1578867987038,
      "id": "e6bd0ce2-cf28-465c-9f90-79ea56e4b43c",
      "body": "test comment nameoe",
      "slug": "title-tpbrqo",
      "author": {
        "username": "commenter-vuctdu",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1578867987038
    },
    {
      "createdAt": 1578867987085,
      "id": "d7d31f50-01ad-4697-bc31-b7d36c3fcee2",
      "body": "test comment 1uc80h",
      "slug": "title-tpbrqo",
      "author": {
        "username": "commenter-vuctdu",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1578867987085
    },
    {
      "createdAt": 1578867987064,
      "id": "a4fbe578-6081-492d-aabf-3d1f3cebea1a",
      "body": "test comment mdwiyk",
      "slug": "title-tpbrqo",
      "author": {
        "username": "commenter-vuctdu",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1578867987064
    }
  ]
}
```
## Delete
### should delete comment
```
DELETE /articles/title-tpbrqo/comments/e5f9e267-fd83-4d56-a265-f17b8ce8d4dc
```
```
200 OK

{}
```
### only comment author should be able to delete comment
```
DELETE /articles/title-tpbrqo/comments/0fca98eb-ecfe-42bb-a294-4cbc108cd0d2
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Only comment author can delete: [commenter-vuctdu]"
    ]
  }
}
```
### should disallow unauthenticated user
```
DELETE /articles/title-tpbrqo/comments/0fca98eb-ecfe-42bb-a294-4cbc108cd0d2
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should disallow deleting unknown comment
```
DELETE /articles/title-tpbrqo/comments/foobar_id
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Comment ID not found: [foobar_id]"
    ]
  }
}
```
# User
## Create
### should create user
```
POST /users

{
  "user": {
    "email": "user1-0.jtsahp6sl2@email.com",
    "username": "user1-0.jtsahp6sl2",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "user1-0.jtsahp6sl2@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuanRzYWhwNnNsMiIsImlhdCI6MTU3ODg2Nzk4NywiZXhwIjoxNTc5MDQwNzg3fQ.Y_ZiXRgkDYjmitg0FtiFcLdyWtbF5KbliFIm74adGoY",
    "username": "user1-0.jtsahp6sl2",
    "bio": "",
    "image": ""
  }
}
```
### should disallow same username
```
POST /users

{
  "user": {
    "email": "user1-0.jtsahp6sl2@email.com",
    "username": "user1-0.jtsahp6sl2",
    "password": "password"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Username already taken: [user1-0.jtsahp6sl2]"
    ]
  }
}
```
### should disallow same email
```
POST /users

{
  "user": {
    "username": "user2",
    "email": "user1-0.jtsahp6sl2@email.com",
    "password": "password"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email already taken: [user1-0.jtsahp6sl2@email.com]"
    ]
  }
}
```
### should enforce required fields
```
POST /users

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "User must be specified."
    ]
  }
}
```
```
POST /users

{
  "user": {
    "foo": 1
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Username must be specified."
    ]
  }
}
```
```
POST /users

{
  "user": {
    "username": 1
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email must be specified."
    ]
  }
}
```
```
POST /users

{
  "user": {
    "username": 1,
    "email": 2
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Password must be specified."
    ]
  }
}
```
## Login
### should login
```
POST /users/login

{
  "user": {
    "email": "user1-0.jtsahp6sl2@email.com",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "user1-0.jtsahp6sl2@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuanRzYWhwNnNsMiIsImlhdCI6MTU3ODg2Nzk4NywiZXhwIjoxNTc5MDQwNzg3fQ.Y_ZiXRgkDYjmitg0FtiFcLdyWtbF5KbliFIm74adGoY",
    "username": "user1-0.jtsahp6sl2",
    "bio": "",
    "image": ""
  }
}
```
### should disallow unknown email
```
POST /users/login

{
  "user": {
    "email": "0.4vke8htcjth",
    "password": "somepassword"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email not found: [0.4vke8htcjth]"
    ]
  }
}
```
### should disallow wrong password
```
POST /users/login

{
  "user": {
    "email": "user1-0.jtsahp6sl2@email.com",
    "password": "0.1s9cw2s7s9w"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Wrong password."
    ]
  }
}
```
### should enforce required fields
```
POST /users/login

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "User must be specified."
    ]
  }
}
```
```
POST /users/login

{
  "user": {}
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email must be specified."
    ]
  }
}
```
```
POST /users/login

{
  "user": {
    "email": "someemail"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Password must be specified."
    ]
  }
}
```
## Get
### should get current user
```
GET /user
```
```
200 OK

{
  "user": {
    "email": "user1-0.jtsahp6sl2@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuanRzYWhwNnNsMiIsImlhdCI6MTU3ODg2Nzk4NywiZXhwIjoxNTc5MDQwNzg3fQ.Y_ZiXRgkDYjmitg0FtiFcLdyWtbF5KbliFIm74adGoY",
    "username": "user1-0.jtsahp6sl2",
    "bio": "",
    "image": ""
  }
}
```
### should disallow bad tokens
```
GET /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
```
GET /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
```
GET /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
```
GET /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
## Profile
### should get profile
```
GET /profiles/user1-0.jtsahp6sl2
```
```
200 OK

{
  "profile": {
    "username": "user1-0.jtsahp6sl2",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
### should disallow unknown username
```
GET /profiles/foo_0.g2kjsa22nzh
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "User not found: [foo_0.g2kjsa22nzh]"
    ]
  }
}
```
### should follow/unfollow user
```
POST /users

{
  "user": {
    "username": "followed_user",
    "email": "followed_user@mail.com",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "followed_user@mail.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImZvbGxvd2VkX3VzZXIiLCJpYXQiOjE1Nzg4Njc5ODcsImV4cCI6MTU3OTA0MDc4N30.6gFf30Z2cmgh-mrHUuQq-Nw2i9ZqTCkWp6T6E31WRh4",
    "username": "followed_user",
    "bio": "",
    "image": ""
  }
}
```
```
POST /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
POST /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
GET /profiles/followed_user
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
GET /profiles/followed_user
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
```
POST /users

{
  "user": {
    "username": "user2-0.igr7agccrip",
    "email": "user2-0.igr7agccrip@mail.com",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "user2-0.igr7agccrip@mail.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIyLTAuaWdyN2FnY2NyaXAiLCJpYXQiOjE1Nzg4Njc5ODcsImV4cCI6MTU3OTA0MDc4N30.MbxUZlSL_u782SkWxmzxPVukzoUOcGpgI3n3c7VYs0M",
    "username": "user2-0.igr7agccrip",
    "bio": "",
    "image": ""
  }
}
```
```
POST /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
DELETE /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
```
DELETE /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
```
DELETE /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
### should disallow following with bad token
```
POST /profiles/followed_user/follow
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
## Update
### should update user
```
PUT /user

{
  "user": {
    "email": "updated-user1-0.jtsahp6sl2@email.com"
  }
}
```
```
200 OK

{
  "user": {
    "username": "user1-0.jtsahp6sl2",
    "email": "updated-user1-0.jtsahp6sl2@email.com",
    "image": "",
    "bio": "",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuanRzYWhwNnNsMiIsImlhdCI6MTU3ODg2Nzk4NywiZXhwIjoxNTc5MDQwNzg3fQ.Y_ZiXRgkDYjmitg0FtiFcLdyWtbF5KbliFIm74adGoY"
  }
}
```
```
PUT /user

{
  "user": {
    "password": "newpassword"
  }
}
```
```
200 OK

{
  "user": {
    "username": "user1-0.jtsahp6sl2",
    "email": "updated-user1-0.jtsahp6sl2@email.com",
    "image": "",
    "bio": "",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuanRzYWhwNnNsMiIsImlhdCI6MTU3ODg2Nzk4NywiZXhwIjoxNTc5MDQwNzg3fQ.Y_ZiXRgkDYjmitg0FtiFcLdyWtbF5KbliFIm74adGoY"
  }
}
```
```
PUT /user

{
  "user": {
    "bio": "newbio"
  }
}
```
```
200 OK

{
  "user": {
    "username": "user1-0.jtsahp6sl2",
    "bio": "newbio",
    "image": "",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuanRzYWhwNnNsMiIsImlhdCI6MTU3ODg2Nzk4NywiZXhwIjoxNTc5MDQwNzg3fQ.Y_ZiXRgkDYjmitg0FtiFcLdyWtbF5KbliFIm74adGoY"
  }
}
```
```
PUT /user

{
  "user": {
    "image": "newimage"
  }
}
```
```
200 OK

{
  "user": {
    "username": "user1-0.jtsahp6sl2",
    "image": "newimage",
    "bio": "newbio",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuanRzYWhwNnNsMiIsImlhdCI6MTU3ODg2Nzk4NywiZXhwIjoxNTc5MDQwNzg3fQ.Y_ZiXRgkDYjmitg0FtiFcLdyWtbF5KbliFIm74adGoY"
  }
}
```
### should disallow missing token/email in update
```
PUT /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
```
PUT /user

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "User must be specified."
    ]
  }
}
```
### should disallow reusing email
```
POST /users

{
  "user": {
    "email": "user2-0.g70pini0nxa@email.com",
    "username": "user2-0.g70pini0nxa",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "user2-0.g70pini0nxa@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIyLTAuZzcwcGluaTBueGEiLCJpYXQiOjE1Nzg4Njc5ODcsImV4cCI6MTU3OTA0MDc4N30.u8jZmivnH477Gts_qtiYAbZf-eBcvVjDMD50KMA5VLU",
    "username": "user2-0.g70pini0nxa",
    "bio": "",
    "image": ""
  }
}
```
```
PUT /user

{
  "user": {
    "email": "user2-0.g70pini0nxa@email.com"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email already taken: [user2-0.g70pini0nxa@email.com]"
    ]
  }
}
```
# Util
## Ping
### should ping
```
GET /ping
```
```
200 OK

{
  "pong": "2020-01-12T22:26:27.979Z",
  "AWS_REGION": "us-east-1",
  "DYNAMODB_NAMESPACE": "dev"
}
```
