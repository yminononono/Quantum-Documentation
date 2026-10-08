
https://github.com/MPh-py/MPh
https://mph.readthedocs.io/en/1.2/index.html
https://www.comsol.com/blogs/automate-modeling-tasks-comsol-api-use-java


- Material を読み込むことはできないらしい... : https://www.comsol.jp/forum/thread/35575/load-materials-from-library-with-java-api


- Phase の animation を作成する
  - https://www.comsol.jp/forum/thread/16184/phase-animation-in-comsol-41
  - Sequence type : Dynamic Data extension
  - Cycle type : Full harmonic

## Trouble Shooting
---

- java の関数の引数の型を決められないというエラーが出る
```python
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
Cell In[12], line 22
---> 22 eig.set("eigsi", 0) 

TypeError: Ambiguous overloads found for com.comsol.clientapi.impl.PropFeatureClient.set(str,int) between:
	public com.comsol.model.PropFeature com.comsol.clientapi.impl.PropFeatureClient.set(java.lang.String,boolean)
	public com.comsol.model.PropFeature com.comsol.clientapi.impl.PropFeatureClient.set(java.lang.String,int)
```
エラーが出る時には、明示的に型を指定する必要がある。
```python
from jpype.types import JInt, JDouble, JArray, JString

eig.set("eigsi", JInt(0)) 
```