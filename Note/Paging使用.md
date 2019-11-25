Jetpack - Paging 简单使用(Java)

想了解下`Paging`使用，就去看了下官方给的`Sample`，大致扫了眼后， ** 我 ** ，这么复杂的，这么一大堆类，都是干嘛的，由于`Kotlin`处于只写过`demo`并没有大量实际用来开发的地步，看着有点不习惯，就想着去找博客看看别人咋说的，大致扫了眼后， ** 我 ** ，我就想了解下怎么用的，怎么都在扯原理，而且还是固定的套路，第一步，给个半成品的`demo`，贴几行代码还不全，然后`PageList`的原理，我连用都不会，我管啥原理我，看不懂啊。。。

于是，我决定，**自己写个尽量简单的仿造官方sample的半成品，不扯细节，不扯原理，我也不管啥优点缺点，主要是我不会，说不清，扯不了，我就是想瞅瞅咋用的而已，会用了之后，我才能去管细节扩展，才能晓得用在项目中替换代价大不大，记录下，能帮到别人最好**

由于我把自己`github`设置成了`private`，就导致没办法直接放链接了

废话说完，上代码，一个简单的只使用网络请求作为数据源的`Demo`

![效果图](https://user-gold-cdn.xitu.io/2019/11/25/16ea0f8712c29dcd?w=320&h=672&f=gif&s=2083274)

***

```xml
implementation "android.arch.paging:runtime:2.1.0"
```

使用的 `2.1.0` 版本

![包目录](https://user-gold-cdn.xitu.io/2019/11/25/16ea101a3cbfdb03?w=338&h=245&f=png&s=23081)

类还是挺多的，用了[wan android](https://www.wanandroid.com/) 的接口`api`，就用了`wan`，数据是以`page`的方式，所以理所当然使用了`Paging`的`PageKeyedDataSource`来获取数据

重点有两个东西`WanDataSource`和`WanVm`，但是需要提前知道`LiveData和ViewModel`的基本用法

***

## 1.0 布局

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".module.widget.paging.PagingActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/paging_rv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scrollbars="vertical"
        app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <!--Loading-->
    <androidx.core.widget.ContentLoadingProgressBar
        android:id="@+id/paging_pb"
        style="?android:attr/progressBarStyleLarge"
        android:layout_width="60dp"
        android:layout_height="60dp"
        android:visibility="visible"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>

```

超简单，就一个`RecyclerView`一个`ProgressBar`

***

### 底部 Footer 布局

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tool="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <ProgressBar
        android:id="@+id/item_wan_footer_pb"
        android:layout_width="40dp"
        android:layout_height="40dp"
        android:layout_margin="10dp"
        android:visibility="visible"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/item_wan_footer_tv_msg"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dp"
        android:layout_marginBottom="10dp"
        android:paddingLeft="12dp"
        android:paddingRight="12dp"
        android:visibility="gone"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tool:text="Something is error"
        tool:visibility="visible" />

    <Button
        android:id="@+id/item_wan_footer_bt_retry"
        style="@style/Widget.AppCompat.Button.Colored"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_marginBottom="10dp"
        android:text="@string/wan_footer_retry"
        android:visibility="gone"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toBottomOf="@id/item_wan_footer_tv_msg"
        tool:visibility="visible" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

![网络错误时，UI](https://user-gold-cdn.xitu.io/2019/11/25/16ea139979898acd?w=1080&h=2280&f=jpeg&s=398912)


`RecyclerView`的`item`布局不再贴出来


***

## 2.0 Actitiy

```java
public class PagingActivity extends BaseActivity {
    private static final String TAG = PagingActivity.class.getSimpleName();
    public static final String LOAD_TAG = "wan";

    private WanAdapter mAdapter;
    private ContentLoadingProgressBar mPb;
    private boolean mIsLoadInitial = true;
    private Runnable mRetryAction;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_paging);
        initView();
        initProvider();
    }

    @Override
    protected void onStop() {
        super.onStop();
        OkHttpUtils.getInstance().cancelByTag(LOAD_TAG);
    }

    private void initProvider() {
        ViewModelProvider provider = ViewModelProviders.of(this);
        WanVm wanVm = provider.get(WanVm.class);
        wanVm.errorMsgLiveData().observe(this, mAdapter::setErrorMsg);
        wanVm.retryLiveData().observe(this, action -> mRetryAction = action);
        wanVm.networkStateLiveData().observe(this, mAdapter::setNetworkState);


        PagedList.Config pagedListConfig = new PagedList.Config.Builder()
                // 分页加载的数量
                .setPageSize(20)
                // 是否启动PlaceHolders
                .setEnablePlaceholders(false)
                .build();
        LiveData<PagedList<WanItem.DataBean.DatasBean>> wanLiveData =
                new LivePagedListBuilder<>(new WanSourceFactory(wanVm), pagedListConfig).build();
        wanLiveData.observe(this, list -> {
            mAdapter.submitList(list);

            if (mIsLoadInitial) {
                mPb.setVisibility(View.GONE);
                mIsLoadInitial = false;
                Log.e(TAG, "-->" + list.size());
            }
        });
    }

    private void initView() {
        mPb = $(R.id.paging_pb);
        RecyclerView rv = $(R.id.paging_rv);
        rv.addItemDecoration(new DividerItemDecoration(this, RecyclerView.VERTICAL));
        mAdapter = new WanAdapter(new WanDiffCallback());
        // Footer 布局内，发生错误，重试按钮 
        mAdapter.addRetryListener(() -> {
            if (mRetryAction == null) {
                return;
            }
			 // new Thread(mRetryAction).start();
            WorkerRunner.findRunner(PagingActivity.this).execute(mRetryAction);
        });
        rv.setAdapter(mAdapter);
    }
}
```

`initView()`就是初始化`RecyclerView`，和不使用`Paging`一样的套路，只是提供数据的方式需要变

重点在于`initProvider()`方法内，首先是创建了`WanVm`，关联了接收到具体消息时回调操作

`PagedList.Config`，这玩意就是`DataSource`分页数据的配置

```java
  LiveData<PagedList<WanItem.DataBean.DatasBean>> wanLiveData =
                new LivePagedListBuilder<>(new WanSourceFactory(wanVm), pagedListConfig).build()
```

创建了一个`LiveData`，用于获取`WanDataSource`网络请求拿到的结果

***

## 2.1 WanAdapter

```java
public class WanAdapter extends PagedListAdapter<WanItem.DataBean.DatasBean, RecyclerView.ViewHolder> {

    public static final String TAG = WanAdapter.class.getSimpleName();
    private NetworkState mNetworkState;
    private OnRetryListener mRetryListener;
    private String mErrorMsg;

    public WanAdapter(@NonNull DiffUtil.ItemCallback<WanItem.DataBean.DatasBean> diffCallback) {
        super(diffCallback);
        mNetworkState = NetworkState.create();
    }

    public void addRetryListener(@NonNull OnRetryListener listener) {
        mRetryListener = listener;
    }

    public void setErrorMsg(@NonNull String msg) {
        this.mErrorMsg = msg;
    }

    @NonNull
    @Override
    public RecyclerView.ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        if (viewType == R.layout.item_wan_footer_layout) {
            View item = LayoutInflater.from(parent.getContext())
                    .inflate(R.layout.item_wan_footer_layout, parent, false);
            return new FooterHolder(item, mRetryListener);
        } else {
            View item = LayoutInflater.from(parent.getContext())
                    .inflate(R.layout.item_wan_layout, parent, false);
            return new WanHolder(item);
        }
    }

    @Override
    public int getItemViewType(int position) {
        if (hasExtraRow() && position == getItemCount() - 1) {
            // loading
            return R.layout.item_wan_footer_layout;
        } else {
            return R.layout.item_wan_layout;
        }
    }

    @Override
    public void onBindViewHolder(@NonNull RecyclerView.ViewHolder holder, int position) {
        if (footer(holder)) {
            return;
        }

        if (holder instanceof WanHolder) {
            WanHolder wanHolder = (WanHolder) holder;
            WanItem.DataBean.DatasBean wan = getItem(position);
            if (wan == null) {
                return;
            }

            Context context = wanHolder.itemView.getContext();
            // 标题
            wanHolder.tvTitle.setText(wan.getTitle());
            // 时间
            wanHolder.tvTime.setText(wan.getNiceDate());
            // 来自
            String uer = TextUtils.isEmpty(wan.getAuthor()) ? wan.getShareUser() : wan.getAuthor();
            String from = context.getResources().getString(R.string.wan_item_from, uer);
            wanHolder.tvUser.setText(from);
            // 分类
            String cls = wan.getSuperChapterName() + "/" + wan.getChapterName();
            String classification = context.getResources().getString(R.string.wan_item_class, cls);
            wanHolder.tvClass.setText(classification);
        }
    }

    private boolean footer(@NonNull RecyclerView.ViewHolder holder) {
        if (holder instanceof FooterHolder) {
            FooterHolder footerHolder = (FooterHolder) holder;
            if (mNetworkState.isLoadFailed()) {
                footerHolder.pb.setVisibility(View.GONE);
                footerHolder.bt.setVisibility(View.VISIBLE);
                footerHolder.tvMsg.setVisibility(View.VISIBLE);
                footerHolder.tvMsg.setText(mErrorMsg);
            } else {
                footerHolder.pb.setVisibility(View.VISIBLE);
                footerHolder.bt.setVisibility(View.GONE);
                footerHolder.tvMsg.setVisibility(View.GONE);
            }

            return true;
        }

        return false;
    }

    @Override
    public int getItemCount() {
        return super.getItemCount() + (hasExtraRow() ? 1 : 0);
    }

    /** 根据网络请求状态结果控制展示 Footer UI */
    public void setNetworkState(int state) {
        Log.d(TAG, "loading state = " + state);
        int preState = mNetworkState.getState();
        boolean oldEx = hasExtraRow();
        Log.d(TAG, "oldEx = " + oldEx);
        mNetworkState.setState(state);
        boolean newEx = hasExtraRow();
        Log.d(TAG, "newEx = " + newEx);
        if (!ObjectsCompat.equals(oldEx, newEx)) {
            // 说明 Footer UI 需要切换，滑倒了最底部
            if (oldEx) {
                notifyItemRemoved(super.getItemCount());
            } else {
                notifyItemInserted(super.getItemCount());
            }
        } else if (newEx && !ObjectsCompat.equals(preState, state)) {
            // 失败时
            notifyItemChanged(getItemCount() - 1);
        }
    }

    private boolean hasExtraRow() {
        return !mNetworkState.isIdle() && !mNetworkState.isLoaded();
    }

    private static class FooterHolder extends RecyclerView.ViewHolder {

        private ProgressBar pb;
        private TextView tvMsg;
        private Button bt;

        private FooterHolder(@NonNull View itemView, @NonNull OnRetryListener listener) {
            super(itemView);
            pb = itemView.findViewById(R.id.item_wan_footer_pb);
            tvMsg = itemView.findViewById(R.id.item_wan_footer_tv_msg);
            bt = itemView.findViewById(R.id.item_wan_footer_bt_retry);
            bt.setOnClickListener(new IntervalClickListener() {
                @Override
                protected void onWiseClick(View v) {
                    listener.onRetry();
                }
            });
        }
    }
	
	/** 点击重试按钮监听 */
    public interface OnRetryListener {
        void onRetry();
    }

    private static class WanHolder extends RecyclerView.ViewHolder {

        private TextView tvTitle;
        private TextView tvUser;
        private TextView tvClass;
        private TextView tvTime;

        private WanHolder(@NonNull View itemView) {
            super(itemView);
            tvTitle = itemView.findViewById(R.id.item_wan_tv_title);
            tvUser = itemView.findViewById(R.id.item_wan_tv_user_name);
            tvClass = itemView.findViewById(R.id.item_wan_tv_classification);
            tvTime = itemView.findViewById(R.id.item_wan_tv_time);
        }
    }
}
```

继承`PagedListAdapter`，内部提供了一个`submitList(PagedList<T> pagedList)`用于改变数据源`List`

构造方法，需要提供一个`DiffUtil`的比较差异`WanDiffCallback`

***

### WanDiffCallback

```java
public class WanDiffCallback extends DiffUtil.ItemCallback<WanItem.DataBean.DatasBean> {
    @Override
    public boolean areItemsTheSame(@NonNull WanItem.DataBean.DatasBean oldItem,
                                   @NonNull WanItem.DataBean.DatasBean newItem) {
        return oldItem.getTitle().endsWith(newItem.getTitle());
    }

    @Override
    public boolean areContentsTheSame(@NonNull WanItem.DataBean.DatasBean oldItem,
                                      @NonNull WanItem.DataBean.DatasBean newItem) {
        return oldItem.getId() == newItem.getId();
    }
}
```

依据`item bean`自身固有属性，提供两个比较的不同的判定方式，告诉`PagedListAdapter`，`Item`数据有变化

***

`setNetworkState()`方法，用来根据网络请求状态添加或者移除`Footer`

### NetworkState

```java
public class NetworkState {
    private static final int IDLE = -1;
    static final int RUNNING = 0;
    static final int SUCCESS = 1;
    static final int FAILED = 2;

    private int mState = IDLE;

    public static NetworkState create() {
        return new NetworkState();
    }

    private NetworkState() {
    }

    public void setState(int state) {
        if (mState != state) {
            this.mState = state;
        }
    }

    public int getState() {
        return mState;
    }

    boolean isLoaded() {
        return mState == SUCCESS;
    }

    boolean isIdle() {
        return mState == IDLE;
    }

    boolean isLoading() {
        return mState == RUNNING;
    }

    boolean isLoadFailed() {
        return mState == FAILED;
    }
}
```

***

## 2.2 WanVm

```java
public class WanVm extends AndroidViewModel {
    private MutableLiveData<Integer> mNetworkStateLiveData = new MutableLiveData<>();
    private MutableLiveData<String> mErrorMsgLiveData = new MutableLiveData<>();
    private MutableLiveData<Runnable> mRetryLiveData = new MutableLiveData<>();

    public WanVm(@NonNull Application application) {
        super(application);
    }

    void postNetworkState(int state) {
        mNetworkStateLiveData.postValue(state);
    }

    void postErrorMsg(@NonNull String msg) {
        mErrorMsgLiveData.postValue(msg);
    }

    void postRetry(@NonNull Runnable action) {
        mRetryLiveData.postValue(action);
    }

    public MutableLiveData<Integer> networkStateLiveData() {
        return mNetworkStateLiveData;
    }

    public MutableLiveData<String> errorMsgLiveData() {
        return mErrorMsgLiveData;
    }

    public MutableLiveData<Runnable> retryLiveData() {
        return mRetryLiveData;
    }

    @Override
    protected void onCleared() {
        super.onCleared();
    }
}
```

`mNetworkStateLiveData:`滑动过程中，用来发送标示网络请求状态
`mErrorMsgLiveData:`网络请求错误的信息
`mRetryLiveData:`重试的`Runnable`任务

***

### WanSourceFactory

提供了一个`DataSource.Factory`用来创建`DataSource`

```java
public class WanSourceFactory extends DataSource.Factory<Integer, WanItem.DataBean.DatasBean> {

    private WanVm mWanVm;

    public WanSourceFactory(WanVm wanVm) {
        mWanVm = wanVm;
    }

    @Override
    public DataSource<Integer, WanItem.DataBean.DatasBean> create() {
        return new WanDataSource(mWanVm);
    }
}
```

***

## 2.3 WanDataSource

```java
public class WanDataSource extends PageKeyedDataSource<Integer, WanItem.DataBean.DatasBean> {

    private static final String TAG = WanDataSource.class.getSimpleName();

    private WanVm mWanVm;

    private boolean mIsLoadInitial = true;

    WanDataSource(WanVm wanVm) {
        mWanVm = wanVm;
    }

    /** 初始化数据: 第 1 页 */
    @Override
    public void loadInitial(@NonNull LoadInitialParams<Integer> params,
                            @NonNull LoadInitialCallback<Integer, WanItem.DataBean.DatasBean> callback) {
        load(0, new LoadCallback() {
            @Override
            public void onSuccess(@NonNull WanItem wanItem) {
                List<WanItem.DataBean.DatasBean> list = wanItem.getData().getDatas();
                if (list == null || list.isEmpty()) {
                    onFailed("loadInitial() no usable data");
                    return;
                }

                callback.onResult(wanItem.getData().getDatas(), 0, 1);
            }

            @Override
            public void onFailed(@NonNull String msg) {
                Log.d(TAG, msg);
                mWanVm.postRetry(() -> loadInitial(params, callback));
            }
        });
    }

    /** 上滑: 前一页 */
    @Override
    public void loadBefore(@NonNull LoadParams<Integer> params,
                           @NonNull PageKeyedDataSource.LoadCallback<Integer, WanItem.DataBean.DatasBean> callback) {
        // 如果 loadInitial() 从 0 开始，这个方法忽略
    }

    /** 下滑: 后一页 */
    @Override
    public void loadAfter(@NonNull LoadParams<Integer> params,
                          @NonNull PageKeyedDataSource.LoadCallback<Integer, WanItem.DataBean.DatasBean> callback) {
        int page = params.key;
        load(page, new LoadCallback() {
            @Override
            public void onSuccess(@NonNull WanItem wanItem) {
                List<WanItem.DataBean.DatasBean> list = wanItem.getData().getDatas();
                if (list == null || list.isEmpty()) {
                    onFailed("loadBefore() no usable data");
                    return;
                }

                callback.onResult(list, page + 1);
            }

            @Override
            public void onFailed(@NonNull String msg) {
                Log.d(TAG, msg);
                mWanVm.postRetry(() -> loadAfter(params, callback));
            }
        });
    }

    private void load(@IntRange(from = 0) int page, @NonNull LoadCallback callback) {
        if (!mIsLoadInitial) {
            mWanVm.postNetworkState(NetworkState.RUNNING);
        }

        mIsLoadInitial = false;

        OkHttpUtils.<WanItem>get()
                .url(Urls.wan(page))
                .tag(LOAD_TAG)
                .build()
                .execute(new OkWanItemCallback() {
                    @Override
                    public void onSuccess(OkResponse<WanItem> okResponse) {
                        mWanVm.postNetworkState(NetworkState.SUCCESS);
                        callback.onSuccess(okResponse.body());
                    }

                    @Override
                    public void onFailure(OkResponse<WanItem> okResponse) {
                        super.onFailure(okResponse);
                        mWanVm.postNetworkState(NetworkState.FAILED);
                        mWanVm.postErrorMsg(okResponse.msg());
                        callback.onFailed(okResponse.msg());
                    }
                });
    }

    private interface LoadCallback {
        void onSuccess(@NonNull WanItem wanItem);

        void onFailed(@NonNull String msg);
    }
}
```

**`loadInitial()，loadBefore()，loadAfter()`内的处理本身在`Paging`内部回调在子线程**

网络请求，自己看了`OkHttpUtils`后，学着做了一个简单的封装，这里`execute()`是同步的

在`onFailure()`失败方法内，发送了网络请求状态，错误信息

由于`loadInitial()`和`loadAfter()`在失败后，`Retry`重试时，需要不同的参数，就直接用`Runnbale`给包装了下，执行时，直接根据网络请求库线程情况，使用`Handler`或者`Executor`都可以

***

除了网络请求的`item bean`，其他代码基本都贴出来了

`ItemKeyedDataSource,PositionalDataSource,PageKeyedDataSource`的使用场景差别，主要取决于数据源的提供结构，至于怎么分的页，怎么加载何时决定加载的，不知道，需要再多了解

原理了，进阶使用之类的，可以看看[Android 官方架构组件 Paging：分页库的设计美学](https://juejin.im/entry/5b3ec756e51d4519945fa300)

体验很好，滑动起来很顺畅，尤其网络好的情况下，但比较麻烦，之前封装的`Adapter`需要单独再扩展适配下，而且加`Header`和`Footer`，不能直接加，需要做些处理

`Paging`设计的很6，贼6，以后得多花点时间学习了解下

刚开始学用，有错误和不妥的地方，请指出

2019年马上结束了，写点东西，纪念下吧





