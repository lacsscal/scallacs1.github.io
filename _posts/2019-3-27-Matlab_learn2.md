# 矩阵输入 #
---
Array(Vector and Matrix)

	>>a = [1 2 3 4]
	输入一个行向量（row vector）
	>>b = [1; 2; 3; 4]
	输入一个列向量,分号使换行（column vector）
	
	矩阵乘法得内外积
	组合可输入矩阵

**矩阵起始元素为 1 **
排序时先列后行
  
##Colon Opertor##
	>>A = [1:100](1到100，等差为1，包含末尾元素)
	>>A = [1:2:100](设置等差步长为2)
	>>删除行 eg:一个三乘三的矩阵A删除第三行
	>>A(3,:) = []


## Array Concatenation ##
做增广矩阵

	>>A = [1 2;3 4];
	>>B = [9 9;9 9];
	>>F = [A,B]  逗号增广，分号换行

## Array Manipulation ##
矩阵运算

**  + - * / ^ . '  **
** .* ； ./ **相乘/除,只取对应元素
**  '  **转置

## Special Matrix  && The Function ##
特殊矩阵
eye(n) ; zero(n) ; diag(n)


	>>max(A)
	>>sum(A)
	>>mean(A)
	>>sort(A)
	>调用一次函数，对单列操作，调用两层，作用于整个矩阵
	>>sortrows(A):也按照第一列操作
	>>size(A):测长度
	>>find(A == 5)返回位置参数
	


# 下篇:结构化&&自定义函数 #