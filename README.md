## һ.��Դ�ļ�����ָ�룬ת���쳣����������д�Ķ�����ȷ�ģ���ô���ʱ����Ҫ�������������������
1.�����ǵ�Android�����ж��module������£��������ģ��������ģ����������ģ�����Դ�ļ��Ḳ����ģ�����е���Դ��������ģ���ȡ����Դ����ģ�����Դ��
2.�������Դ�ļ���layout,string,color,style�ȣ����ǲ�����id�������ͬģ���id��ͬ�Ļ��������û�����⣬���˰���id��layoutҲ��ͬ�������ͻ�����ģ���id�ˡ�

## ��.��ֻ������һ��Ӧ�ó���ΪʲôApplication��onCreateִ���˶�Σ�
��:������Ӧ�ó����ʱ��linux�е���fork�������ӽ��̣����������̵Ĵ���ռ䣬���Ƹ��������ݿռ䣬��ʱ�ӽ��̻��ø����̵����б�����һ�ݿ�����������ʱ���������ܻ������µĽ��̣���ôҲ��ִ�н�������Application�Ĵ��룬���Ի�ִ�ж���ˡ�

## ��.View.setVIsibility��Gone����ʱ�򣬲������ã����߳���gone����һ��ؼ�Ϊ��ɫ��
��:�޸Ĳ��ֵ����ã�
```
    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <LinearLayout
            android:id="@+id/vis_or_gone"//ͨ�����id������Visible����Gone
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@color/white"
            android:visibility="gone">

            //���������ҪVisible��Gone�Ĳ���
        </LinearLayout>
    </RelativeLayout>
```

## ��.��Ϊ�ֻ��������ֻ���popupwindow�а���EditText��ʱ�򣬵�EditText��ȡ���㣬����popupwindow�ı�������͸����
��:�����Ҫ������popupwindow��contentView�ı�������Ϊ��Ҫ����ɫ������contentView�а������ӿؼ�������������Ŀؼ����������ϲ���ʾ�Ŀؼ���Ҳ��Ҫ����������Ҫ�ı���ɫ������popwindow�Ͳ�����͸���ˡ�


## ��.Tablayout + ViewPager + fragment �л�ʱ�������ڲ�����?
��������д�Լ���fragmentAdapter��ʱ�򣬽�tag��position�����������������
```
public class BaseFragmentAdapter extends TabFragmentAdapter {
    private FragmentManager mFragmentManager;
    private SparseArray<String> mFragmentTags;

    public BaseFragmentAdapter(@NonNull FragmentManager fm, @NonNull List<String> titles, @NonNull List<Fragment> fragments) {
        super(fm, titles, fragments);
        mFragmentManager = fm;
        mFragmentTags = new SparseArray<>();
    }

    @Override
    public Object instantiateItem(ViewGroup container, int position) {
        Object object = super.instantiateItem(container, position);
        if (object instanceof Fragment) {
            Fragment fragment = (Fragment) object;
            String tag = fragment.getTag();
            mFragmentTags.append(position, tag);
        }
        return object;
    }

    public Fragment getFragment(int position){
        String tag = mFragmentTags.get(position);
        if(StringUtil.isStringEmpty(tag)){
            return null;
        }
        return mFragmentManager.findFragmentByTag(tag);
    }

}
```
Ȼ����vp�л��Ļص������е��ã�
```
 mViewpager.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {
            @Override
            public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {

            }

            @Override
            public void onPageSelected(int position) {
                Fragment currentFragment = mTabFragmentAdapter.getFragment(position);
                if ((0 == position || position == mTabFragmentAdapter.getCount() - 1) && null != currentFragment) {
                    currentFragment.onResume();
                }
            }

            @Override
            public void onPageScrollStateChanged(int state) {

            }
        });
```
������ǵ���onResume��ʲô���ڣ���������Ҳ�ǿ��Դ���ġ�

## ��.Android�ж�ĳ��ViewƵ������Visible��Gone��ʱ���е�ʱ������GoneȴGone�����������
������������������ΪView��û�м�����Լ��Ŀ�ߣ�����Gone����������һ����������·�ʽ��
```
    YouView.post(new Runnable() {
            @Override
            public void run() {
                rlOptionByEmployee.setVisibility(View.GONE);
                }
            });
```
����������������View��״̬��gone֮�����������µĴ��룺
```
    YouView.requestLayout();
    YouView.invalide();
```

## ��.Android��Popupwindow��7.0���ϵ��豸����showAsDropDown��ʱ�򣬵���������match_parent��ʱ�򣬻�ȫ��������
��:���Կ�����[ר��Ϊ֧��7.0���ϵ��豸��ʾ��popupwindow](https://github.com/WelliJohn/PopupWindowSet/blob/master/popwindowset/src/main/java/wellijohn/org/popwindowset/DropDownPopupWindow.java)��