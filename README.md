# testui


public class MyAdapter extends BaseAdapter {
    private List<String> Datas;
    private Context mContext;

    public MyAdapter(List<String> datas, Context mContext) {
        Datas = datas;
        this.mContext = mContext;
    }

    @Override
    public int getCount() {
        return Datas.size();
    }

    @Override
    public Object getItem(int i) {
        return Datas.get(i);
    }

    @Override
    public long getItemId(int i) {
        return i;
    }

    @RequiresApi(api = Build.VERSION_CODES.JELLY_BEAN_MR2)
    @Override
    public View getView(int i, View view, ViewGroup viewGroup) {
        View view1 = LayoutInflater.from(mContext).inflate(R.layout.image_item,viewGroup,false);
        ImageView imageView1 = (ImageView) view1.findViewById(R.id.image1);
//        imageView1.setImageResource(R.drawable.testpic1);
//        ImageView imageView2 = (ImageView) view1.findViewById(R.id.image);
//        imageView2.setImageResource(R.drawable.testpic1);
//        ImageView imageView3 = (ImageView) view1.findViewById(R.id.image);
//        imageView3.setImageResource(R.drawable.testpic1);
        Bitmap bm = BitmapFactory.decodeFile(Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_PICTURES).getPath() + "/" + (i+1) + ".jpg");
        Log.d("mzl", "getView i ：" + i + " view: " + view1);
        imageView1.setImageBitmap(bm);
        TextView textView1 = (TextView) view1.findViewById(R.id.word);
        textView1.setText(Datas.get(i));
//        此处需要返回view 不能是view中某一个
        imageView1.setImageResource(R.drawable.testframeanimation);
        AnimationDrawable ad = (AnimationDrawable) imageView1.getDrawable();
        //ad.start();

        Animation animation = AnimationUtils.loadAnimation(mContext, R.anim.testscale);
        //imageView1.setOnClickListener((v) -> {imageView1.startAnimation(animation);});

//        ObjectAnimator transXAnim = ObjectAnimator.ofFloat(view1, "translationX", 100, 400);
//        ObjectAnimator transYAnim = ObjectAnimator.ofFloat(view1, "translationY", 100, 750);
//        AnimatorSet set = new AnimatorSet();
//        set.playTogether(transXAnim, transYAnim);
//        set.setDuration(3000);
        //set.start();
        TextView textView = new TextView(mContext);
        textView.setText("hahahaha");
        textView.setWidth(300);
        textView.setHeight(300);
        view1.getOverlay().add(createViewBitmap(textView));
        view1.setOnClickListener((v) -> {
            //textView.setText("nononono");
            playItemAnimation(v);
        });
        return view1;
    }

    public Drawable createViewBitmap(View view) {
        Bitmap bitmap = Bitmap.createBitmap(500, 500, Bitmap.Config.ARGB_8888);
        Canvas canvas = new Canvas(bitmap);
        view.draw(canvas);
        Drawable drawable = new BitmapDrawable(view.getResources());
//        drawable.setBounds();
        return drawable;
    }

    private void playItemAnimation(View view) {
        ValueAnimator alphaAnimation = ValueAnimator.ofFloat(1, 0);
        alphaAnimation.setDuration(500);

        ValueAnimator heightAnimation = ValueAnimator.ofFloat(0, 1);
        heightAnimation.setDuration(500);

        alphaAnimation.setInterpolator(new AccelerateInterpolator());
        heightAnimation.setInterpolator(new DecelerateInterpolator());

        AnimatorSet set = new AnimatorSet();
        set.play(alphaAnimation).with(heightAnimation);
        alphaAnimation.addUpdateListener(new AlphaListener(view));
        heightAnimation.addUpdateListener(new HeightListener(view));
        set.start();
    }

    static class AlphaListener implements ValueAnimator.AnimatorUpdateListener {
        View view;

        public AlphaListener(View view) {
            this.view = view;
        }

        @Override
        public void onAnimationUpdate(ValueAnimator animation) {
            float animatedValue = (float)animation.getAnimatedValue();
            view.setAlpha(animatedValue);
        }
    }

    static class HeightListener implements ValueAnimator.AnimatorUpdateListener {
        View view;

        public HeightListener(View view) {
            this.view = view;
        }

        @Override
        public void onAnimationUpdate(ValueAnimator animation) {
            float animatedValue = (float)animation.getAnimatedValue();
            view.setTranslationY(-animatedValue * view.getHeight());
        }
    }
