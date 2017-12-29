#include "iostream"
#include <string>
#include <iomanip>
using namespace std;
//define of node
template<class Type>
class node
{
public:
	node();
	~node();
	Type nodeValue;
	node<Type> *left, *right;
	node(const Type &item, node<Type> *lptr = NULL, node<Type> *rptr = NULL) :
		nodeValue(item), left(lptr), right(rptr)
	{}
};
template<class Type>
node<Type>::node()
{
	left = NULL;
	right = NULL;
}
template<class Type>
node<Type>::~node()
{

}
//define of BinaryTree
template<class Type>
class BinaryTree
{
public:
	BinaryTree();
	~BinaryTree();
	void MakeTree(Type item, BinaryTree<Type> leftTree, BinaryTree<Type> rightTree);
	void Code();
	void Clear();
	string Huffmancode[100];
private:
	node<Type> *root;
	string code;
	int count;
	void Destroy(node<Type> *p);
	void Display(node<Type> *p);
};
template<class Type>
BinaryTree<Type>::BinaryTree()
{
	root = NULL;
	count = 0;
}
template<class Type>
BinaryTree<Type>::~BinaryTree()
{
}
template<class Type>
void BinaryTree<Type>::Destroy(node<Type> *p)
{
	if (p != NULL)
	{
		Destroy(p->left);
		Destroy(p->right);
		delete p;
		p = NULL;
	}
}
template<class Type>
void BinaryTree<Type>::Clear()
{
	Destroy(root);
}
template<class Type>
void BinaryTree<Type>::MakeTree(Type item, BinaryTree<Type> leftTree, BinaryTree<Type> rightTree)
{
	root = new node<Type>(item, leftTree.root, rightTree.root);
}
template<class Type>
void BinaryTree<Type>::Code()
{
	Display(root);
}
template<class Type>
void BinaryTree<Type>::Display(node<Type> *p)
{
	if (p->left == NULL && p->right == NULL)
	{
		//cout << p->nodeValue << ": " << code << endl;
		Huffmancode[p->nodeValue] = code;
		count++;
		//child node get Huffman code then return to parent node
		code = code.substr(0, code.length() - 1);
	}
	if (p->left != NULL)
	{
		code += "0";	
		Display(p->left);
	}
	if (p->right != NULL)
	{
		code += "1";
		Display(p->right);
		//traversal right subtree return to parent node
		code = code.substr(0, code.length() - 1);
	}
}

//define of MinHeap
template<class Type>
class MinHeap
{
private:
	Type *Heap;
	int size;
	int capacity;
	void trimUp(int top); 
	void trimDown(int top, int end);
public:
	MinHeap(int n);
	~MinHeap();
	bool Insert(const Type &item);
	Type DeleteMin();
	Type GetMin();
	bool IsEmpty();
	void Clear();
};
template<class Type>
MinHeap<Type>::MinHeap(int n)
{
	capacity = 1000;
	Heap = new Type[capacity];
	size = 0;
}
template<class Type>
MinHeap<Type>::~MinHeap()
{
	delete[]Heap;
}
template<class Type>
void MinHeap<Type>::trimUp(int top)
{
	int i = top, j = (top - 1) / 2;
	Type temp = Heap[i];
	//move element which larger than Heap[0] to head start from head
	while (i > 0)
	{
		if (Heap[j] <= temp)
			break;
		else
		{
			Heap[i] = Heap[j];
			i = j;
			j = (j - 1) / 2;
		}
	}
	Heap[i] = temp;
}
template<class Type>
void MinHeap<Type>::trimDown(int top, int end)
{
	int i = top, j = 2 * top + 1;
	Type temp = Heap[i];
	//move element which larger than Heap[0] to head start from end
	while (j <= end)
	{
		if ((j < end) && (Heap[j] > Heap[j + 1]))
			j++;
		if (temp <= Heap[j])
			break;
		else
		{
			Heap[i] = Heap[j];
			i = j;
			j = 2 * j + 1;
		}
	}
	Heap[i] = temp;
}
template<class Type>
bool MinHeap<Type>::Insert(const Type &item)
{
	if (size == capacity)
		return false;
	Heap[size] = item;
	trimUp(size);
	size++;
	return true;
}
template<class Type>
Type MinHeap<Type>::DeleteMin()
{
	Type item = Heap[0];
	Heap[0] = Heap[size - 1];
	size--;
	trimDown(0, size - 1);
	return item;
}
template<class Type>
Type MinHeap<Type>::GetMin()
{
	return Heap[0];
}
template<class Type>
bool MinHeap<Type>::IsEmpty()
{
	return size == 0;
}
template<class Type>
void MinHeap<Type>::Clear()
{
	size = 0;
}
//define of Huffman
template<class Type>
class Huffman
{
	friend BinaryTree<int> HuffmanTree(Type[], int);
public:
	operator Type () const
	{
		return weight;
	}

	BinaryTree<int> tree;
	Type weight;
};
template<class Type>
BinaryTree<int> HuffmanTree(Type f[], int n)
{
	Huffman<Type> *w = new Huffman<Type>[n];
	BinaryTree<int> z, zero;
	//init w
	for (int i = 0; i < n; i++)
	{
		z.MakeTree(i, zero, zero);
		w[i].weight = f[i];
		w[i].tree = z;
	}
	//init Heap
	MinHeap<Huffman<Type>> Q(1);
	for (int i = 0; i < n; i++)
		Q.Insert(w[i]);
	//merge trees
	Huffman<Type> x, y;
	for (int i = 0; i < n - 1; i++)
	{
		x = Q.DeleteMin();
		y = Q.DeleteMin();
		z.MakeTree(0, x.tree, y.tree);
		x.weight += y.weight;
		x.tree = z;
		Q.Insert(x);
	}
	//last merge
	x = Q.DeleteMin();
	delete[] w;
	return x.tree;
}
int main(int argc, char* argv[])
{
	int count = 0, j ,i;
	string str;
	cout << "Please input your sentence: " << endl;
	cin.sync();
	getline(cin, str);
	int n = str.length();
	char c[100];
	int f[100] = { 0 };
	cout << endl;
	//print reszult
	for (i = 0; i < n; i++)
	{
		for (j = 0 ; j < count; j++)
		{
			if (str[i] == c[j])
				break;
		}
		if (j < count)
			f[j]++;
		else
		{
			c[j] = str[i];
			f[j]++;
			count++;
		}
	}	
	cout << "Here is your HuffmanCode list:" << endl;
	BinaryTree<int> t = HuffmanTree(f, count);
	t.Code();
	cout << setw(2) << "No" << setw(5) << "Char" << setw(6) << "Freq" << setw(14) << "Huffmancode" << endl;
	for (i = 0; i < count; i++)
	{
		cout << setw(2) << i+1 << setw(5)<< c[i] << setw(6) << f[i] << setw(14) << t.Huffmancode[i] << endl;
	}
	cout << endl << "Here is your HuffmanCode:" << endl;
	for (i = 0; i < n; i++)
	{
		for (j = 0; j < count; j++)
		{
			if (str[i] == c[j])
			{
				cout << t.Huffmancode[j];
			}
		}
	}
	cout << endl;
	t.Clear();
	system("pause");
	return 0;
}
