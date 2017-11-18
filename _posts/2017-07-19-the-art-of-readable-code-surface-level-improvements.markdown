---
layout: default
title: "The art of readable code: Surface level improvements"
date: 2017-07-19
categories: ruby
---

> Part One of The art of readable code: Surface level improvements

> KEY IDEA: PACK INFORMATION INTO YOUR NAME.

## I. Choose specific words, avoiding "empty" words.

Example: GetPage() => "Get" cannot show you the really meaning of behavior. "Get" is empty word, you donot know "The Page" is get from local or database or internet.

Solution: use DownloadPage(), or FetchPage() instead of GetPage().

## II. Finding more "colorful" words.

- English is a Rich language.
- Donot be afraid to use "colorful" words.

Example:

| Words         | Alternatives  |
| ------------- | ------------- |
| send          | deliver, dispatch, announce, distribute |
| find          | search, extract, locate, recover |
| start         | launch, create, begin, open |
| make          | create, setup, build, generate, compose |

## III. Avoid generic names like "tmp" and "retval".

- Use the name that describes the variable values
- The name "tmp" should be used only in cases when being short-lived-block

### IV. Loop Iterators.

Assume we have arrays: clubs , members and cars

for(int i = 0; i < clubs.size; i++) {
  for(int j = 0; j < members.size; j++) {
    for(int k = 0; k < members.size; k++) {
      <!-- the lots of code here -->
      ...
      ...
      ...
      i = j;
      j = k;
      k = i;
      wtf? the problem is: what are "i j k" mean ?
    }
  }
}

You should choose name of iterator (i, j, k) associate with the array (clubs, members, cars)
Example:
* i => club_i
* j => member_i
* k => car_k
