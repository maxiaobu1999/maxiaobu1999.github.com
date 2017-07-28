##Fragment总结
*   [Fragment生命周期](https://maxiaobu1999.github.io/html5/heima/README.html)
   ![生命周期图](http://img-1253423006.costj.myqcloud.com/fragment_lifecycle.png)

   * onAttach：onAttach()回调将在Fragment与其Activity关联之后调用。需要使用Activity的引用或者使用Activity作为其他操作的上下文，将在此回调方法中实现。
    需要注意的是：将Fragment附加到Activity以后，就无法再次调用setArguments()——除了在最开始，无法向初始化参数添加内容。
   * onCreate(Bundle savedInstanceState)：此时的Fragment的onCreat回调时，该fragmet还没有获得Activity的onCreate()已完成的通知，所以不能将依赖于Activity视图层次结构存在性的代码放入此回调方法中。在onCreate()回调方法中，我们应该尽量避免耗时操作。此时的bundle就可以获取到activity传来的参数
   * onCreateView(LayoutInflater inflater, ViewGroup container,
     Bundle savedInstanceState)： 其中的Bundle为状态包与上面的bundle不一样。
     注意的是：不要将视图层次结构附加到传入的ViewGroup父元素中，该关联会自动完成。如果在此回调中将碎片的视图层次结构附加到父元素，很可能会出现异常。
     这句话什么意思呢？就是不要把初始化的view视图主动添加到container里面，以为这会系统自带，所以inflate函数的第三个参数必须填false，而且不能出现container.addView(v)的操作。
   * onActivityCreated：onActivityCreated()回调会在Activity完成其onCreate()回调之后调用。在调用onActivityCreated()之前，Activity的视图层次结构已经准备好了，这是在用户看到用户界面之前你可对用户界面执行的最后调整的地方。
     强调的point：如果Activity和她的Fragment是从保存的状态重新创建的，此回调尤其重要，也可以在这里确保此Activity的其他所有Fragment已经附加到该Activity中了
   * onDestroyView:该回调方法在视图层次结构与Fragment分离之后调用。
   * onDestroy：不再使用Fragment时调用。（备注：Fragment仍然附加到Activity并任然可以找到，但是不能执行其他操作）
   * onDetach：Fragme生命周期最后回调函数，调用后，Fragment不再与Activity绑定，释放资源。
   
*   [Fragment之间的通讯](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 在Fragment中调用Activity中的方法getActivity()
    * 在Activity中调用Fragment中的方法 接口回调
    * 在Fragment中调用Fragment中的方法findFragmentById
*   [Fragment管理器：FragmentManager](https://maxiaobu1999.github.io/html5/heima/README.html)
*   [Fragment的replace、add、remove方法](https://maxiaobu1999.github.io/html5/heima/README.html)
*   [Fragment加载到Activity的两种方式](https://maxiaobu1999.github.io/html5/heima/README.html)
    * 添加Fragment到Activity布局文件中
    * 动态在activity中添加fragment
    
          //步骤一：添加一个FragmentTransaction实例
          FragmentManager fragmentManager = getFragmentManager();
          FragmentTransaction transaction = fragmentManager.beginTransaction();
  
          //步骤二：用add()方法加上fragment的对象rightFragment
          RightFragment rightFragment = new RightFragment();
          transaction.add(R.id.right_contrainer, rightFragment, "rightFragment");
          transaction.addToBackStack("rightFragment");
  
          //步骤三:调用commit()方法使得FragmentTransaction实例的改变生效
          transaction.commit();
                
*   [FragmentPagerAdapter与FragmentStatePagerAdapter区别](https://maxiaobu1999.github.io/html5/heima/README.html)

    * FragmentPagerAdapter 用detach((Fragment)object) ui分离 并不回收
    
            @Override
            public void destroyItem(ViewGroup container, int position, Object object) {
                if (mCurTransaction == null) {
                    mCurTransaction = mFragmentManager.beginTransaction();
                }
                if (DEBUG) Log.v(TAG, "Detaching item #" + getItemId(position) + ": f=" + object
                        + " v=" + ((Fragment)object).getView());
                mCurTransaction.detach((Fragment)object);
            }
     
    * FragmentStatePagerAdapter用remove适合页面多
   
           @Override
           public void destroyItem(ViewGroup container, int position, Object object) {
               Fragment fragment = (Fragment) object;
      
               if (mCurTransaction == null) {
                   mCurTransaction = mFragmentManager.beginTransaction();
               }
               if (DEBUG) Log.v(TAG, "Removing item #" + position + ": f=" + object
                       + " v=" + ((Fragment)object).getView());
               while (mSavedState.size() <= position) {
                   mSavedState.add(null);
               }
               mSavedState.set(position, fragment.isAdded()
                       ? mFragmentManager.saveFragmentInstanceState(fragment) : null);
               mFragments.set(position, null);
      
               mCurTransaction.remove(fragment);
           }
