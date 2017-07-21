##Activity总结
    作用：与用户交互的界面
*   [Fragment生命周期](https://maxiaobu1999.github.io/html5/heima/README.html)
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
