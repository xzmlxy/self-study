
submission['accuracy_group'] = test.groupby('installation_id').last()['title'].map(labels_map).reset_index(drop=True)

 https://www.jianshu.com/p/ac45e8d168ea
