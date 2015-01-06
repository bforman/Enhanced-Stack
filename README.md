Enhanced-Stack
==============

A functioning, generic enhanced stack class written in Java. Nodes with accompanying data can be pushed and popped off the stack, and the minimum item in the stack is kept track of. The stack inherits from Iterable and has an iterator inner class to assists in iterating through the sequence of items the stack consists of.



import java.util.Iterator;
import java.util.NoSuchElementException;
/**
 * EnhancedStack class, generic class. Assisted by: Ian Matthews, and Nick Araya
 * @author Ben Forman
 * @version 1.0
 *
 * @param <T>
 */

public class EnhancedStack <T extends Comparable<?super T>> implements Iterable<T>
{
    private int size;
    private Node<T> top;
    public Node<T> minimum;

    
    /**
     * inner Node class creates nodes and handles and updates its data.
     * @author Ben Forman
     *
     * @param <T>
     */
    private static class Node<T>
    {
        public T element;
        public Node<T> next;
        public Node<T> small;
        /**
         * Node constructor pairs data with a newly added node in the sequence.
         * @param element
         * @param next
         */
        public Node(T element, Node<T> next)
        {
            this.element = element;
            next = null;
        }
        /**
         * I found myself not needing to use the other node constructor,
         * or the accessors and mutators so I commented them out.
         */
        //        public Node(T e, Node<T> n, Node<T> s)
        //        {
        //            element = e;
        //            next = n; 
        //            small = s;
        //        }
        //
        //        public void setNext()
        //        {
        //            this.next = next;
        //        }
        //
        //        public Node<T> getNext()
        //        {
        //            return next;
        //        }
        //
        //        public void setSmall()
        //        {
        //            this.small = small;
        //        }

//        public Node<T> getSmall()
//        {
//            return small;
//
//        }
    }
    /**
     * Constructor for EnhancedStack sets the top and minimum references to null, and size to 0.
     */
    public EnhancedStack()
    {
        size = 0;
        top = null;
        minimum = null;
    }
    /**
     * top returns a reference to the top of the stack.
     * @return top returns that reference.
     */
    public Node<T> top()
    {
        return top;
    }
    /**
     * size returns the number of nodes in the stack.
     * @return size returns that number.
     */
    public int size()
    {
        return size;
    }
    /**
     * isEmpty verifies if the stack if empty or not.
     * @return boolean returns that verification.
     */
    public boolean isEmpty()
    {
        if (size == 0)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
    /**
     * minimum returns the data that the node minimum references.
     * @return
     */
    public T minimum()
    {
        return minimum.element;
    }
    /**
     * push adds a node to the top of the stack.
     * specific boundary cases are checked to decide how minimum and small should be set.
     * @param data is the data to be linked to the newly added node.
     */
    public void push(T data)
    {
        Node<T> n = new Node<T>(data, top);
        if (isEmpty())
        {
            n.small = null;
            minimum = n;
        }
        else if (top.next == null)
        {
            n.small = top;
        }
        else if (top.element.compareTo(top.small.element) < 0)
        {
            n.small = top;
        }
        else
        {
            n.small = top.small;
        }
        top = n;

        size++;
        if (size > 1)
        {
            if (top.element.compareTo(top.small.element) > 0)
            {
                minimum = top.small;
            }
            else
            {
                minimum = top;
            }
        }
    }
    /**
     * pop removes the top node and extracts the data and returns said data.
     * specific boundary cases are checked to decide how minimum and small should be set.
     * @return t.element returns that data.
     */
    public T pop()
    {
        size--;
        Node<T> t = top;
        if (!isEmpty() && top.next != null)
        {
            if (size == 1)
            {
                minimum = null;
            }
            else if (size == 2)
            {
                minimum = top.next;
            }
            else if (top.next.element.compareTo(top.next.small.element) > 0)
            {
                minimum = top.next.small;
            }
            else
            {
                minimum = top.next;
            }
            top = top.next;
        }
        return t.element;


    }
    /**
     * iterator creates an iterator.
     */
    public Iterator<T> iterator()
    {
        return new EnhancedStackIt();
    }
    /**
     * inner iterator class implements the necessary methods.
     * @author Ben Forman
     *
     */
    private class EnhancedStackIt implements Iterator<T>
    {
        Node<T> node;
        T data;
        /**
         * constructor for the stack iterator.
         */
        private EnhancedStackIt()
        {
            node = top;
        }
        /**
         * remove removes a node.
         */
        public void remove()
        {
            throw new UnsupportedOperationException();
        }
        /**
         * hasNext verifies if there is another node in the set.
         */
        public boolean hasNext()
        {
            return (node != null);

        }
        /**
         * next calls the next node in the stack.
         */
        public T next()
        {
            if (!hasNext())
            {
                throw new NoSuchElementException("No more elements");
            }
            data = node.element;
            node = node.next;
            return data;

        }
    }
    
}
