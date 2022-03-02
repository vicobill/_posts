Logic:
- foreach -> for, foreach会生成Enumerator，分配空间
- String -> StringBuilder
- GameObject.Find* -> 预存
- Instantiate -> Pool
- Update -> event，或跳帧计算
- box/unbox -> 避免
- GetComponentsInChildren -> 预存


- Texture -> Texture Sheet
- Material Instance -> sharedMaterial
- 