## java的 重写HashCode和equals

## 关于Object源码
###分析可知
##
	对象在不重写的情况下使用的是Object的equals方法和hashcode方法，
	从Object类的源码我们知道，默认的equals 判断的是两个对象的引用指向的是不是同一个对象；
	而hashcode也是根据对象地址生成一个整数数值；
	另外我们可以看到Object的hashcode()方法的修饰符为native,表明该方法是否操作系统实现，
	java调用操作系统底层代码获取哈希值。

##
	public class Object {
 
	public native int hashCode();
 
	    /**
	     * Indicates whether some other object is "equal to" this one.
	     * <p>
	     * The {@code equals} method implements an equivalence relation
	     * on non-null object references:
	     * <ul>
	     * <li>It is <i>reflexive</i>: for any non-null reference value
	     *     {@code x}, {@code x.equals(x)} should return
	     *     {@code true}.
	     * <li>It is <i>symmetric</i>: for any non-null reference values
	     *     {@code x} and {@code y}, {@code x.equals(y)}
	     *     should return {@code true} if and only if
	     *     {@code y.equals(x)} returns {@code true}.
	     * <li>It is <i>transitive</i>: for any non-null reference values
	     *     {@code x}, {@code y}, and {@code z}, if
	     *     {@code x.equals(y)} returns {@code true} and
	     *     {@code y.equals(z)} returns {@code true}, then
	     *     {@code x.equals(z)} should return {@code true}.
	     * <li>It is <i>consistent</i>: for any non-null reference values
	     *     {@code x} and {@code y}, multiple invocations of
	     *     {@code x.equals(y)} consistently return {@code true}
	     *     or consistently return {@code false}, provided no
	     *     information used in {@code equals} comparisons on the
	     *     objects is modified.
	     * <li>For any non-null reference value {@code x},
	     *     {@code x.equals(null)} should return {@code false}.
	     * </ul>
	     * <p>
	     * The {@code equals} method for class {@code Object} implements
	     * the most discriminating possible equivalence relation on objects;
	     * that is, for any non-null reference values {@code x} and
	     * {@code y}, this method returns {@code true} if and only
	     * if {@code x} and {@code y} refer to the same object
	     * ({@code x == y} has the value {@code true}).
	     * <p>
	     * Note that it is generally necessary to override the {@code hashCode}
	     * method whenever this method is overridden, so as to maintain the
	     * general contract for the {@code hashCode} method, which states
	     * that equal objects must have equal hash codes.
	     *
	     * @param   obj   the reference object with which to compare.
	     * @return  {@code true} if this object is the same as the obj
	     *          argument; {@code false} otherwise.
	     * @see     #hashCode()
	     * @see     java.util.HashMap
	     */
	    public boolean equals(Object obj) {
	        return (this == obj);
	    }
	}

### 使用HashCode 重写原因
HashSet如何保持唯一性？
当我们将一个对象放入一个HashSet时，它使用该对象的hashcode值来确定一个元素是否已经在该集合中。

每个散列码值对应于某个块位置，该块位置可以包含计算出的散列值相同的各种元素。但是具有相同hashCode的两个对象可能不相等。

因此，将使用equals（）方法比较同一存储桶中的对象。

