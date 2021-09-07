Binary search
=============

"Although the basic idea of binary search is comparatively straightforward, the
details can be suprisingly tricky" -- Donald Knuth

Basic idea
----------

Binary search can check if a target value exists in a sorted array.

.. code-block:: cpp

   int l = 0, r = n-1;
   while (l <= r) {
      int m = (l+r)/2;
      if (a[m] == target) {
         // target found at index m!
      }
      if (a[m] > target) r = m-1;
      else l = m+1;
   }

The intuition is apparent for the most part. If the middle element is too large
we chop the upper bound by r = m-1, and if the middle is too small we chop the
lower bound shifting our new middle to something larger.

The less than or equal to loop condition is important. What if we ommited
the or equal to leaving l < r?

.. code-block::

   // Let's search for 9 in [5, 7, 8, 9]
   l = 0, r = 3
      m = 1
      a[1] = 7 < 9
      l = 2
   l = 2, r = 3
      m = 2
      a[2] = 8 < 9
      l = 3
   // No! The search exits before reaching the target
   // change l < r to l <= r to finish searching
   l = 3, r = 3
      m = 3
      a[3] = 9

Details
-------

Binary search can find *closest* values by taking advantage of exit state. 
Make target = 6 and find the first element equal to 6 or greater. 

.. code-block::

   // First element greater than or equal to 6 in [5, 7, 8, 9]
   l = 0, r = 3
      m = 1
      a[1] = 7 > 6
      r = 0
   l = 0, r = 0
      m = 0
      a[0] = 5 < 6
      l = 1
   // After loop, l = 1 holds the answer

Make target 10 and find the first element equal to 10 or greater

.. code-block::

   // First element >= 10 in [5, 7, 8, 9]
   l = 0, r = 3
      m = 1
      a[1] = 7 < 10
      l = 2
   l = 2, r = 3
      m = 2
      a[2] = 8 < 10
      l = 3
   l = 3, r = 3
      m = 3
      a[3] = 9 < 10
      l = 4
   // After loop, l = 4 is out of bounds as expected

Find where the value of a function changes 
------------------------------------------
