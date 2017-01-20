#CLWeeklyCalendarView

CLWeeklyCalendarView is a scrollable weekly calendarView for iPhone. It is easy to use and customised.


![alt tag](https://github.com/clisuper/CLWeeklyCalendarView/blob/master/screenshot.PNG)

## Installation
* Drag the `CLWeeklyCalendarViewSource` folder into your project.


## Demo Usage

**Please download all of the files in the repo and run it directly to have a look.**



## Initialize 
Using CLWeeklyCalendarViewSource in your app will usually look as simple as this :


```objective-c

//Initialize
-(CLWeeklyCalendarView *)calendarView
{
    if(!_calendarView){
        _calendarView = [[CLWeeklyCalendarView alloc] initWithFrame:CGRectMake(0, 0, self.view.bounds.size.width, 100)];
        _calendarView.delegate = self;
    }
    return _calendarView;
}

//Add it into parentView
[self.view addSubview:self.calendarView];

```

## Delegate

After the date in the calendar has been selected , following delegate function will be fired

```objective-c
//After getting data callback
-(void)dailyCalendarViewDidSelect:(NSDate *)date
{
    //You can do any logic after the view select the date
}
```

You can delegate to tell the calenderView scrollTo specified date by using following delegate function

```objective-c
- (void)redrawToDate: (NSDate *)dt;
```

## Customisation

**Please be aware customisation delegate function is optional, if u do not apply it, it will just fire the default value.

Using CLWeeklyCalendarViewDelegate to customise the behaviour

Following customisation key is allowed

```objective-c
// Keys for customize the calendar behavior
extern NSString *const CLCalendarWeekStartDay;    //The Day of weekStart from 1 - 7 - Default: 1

extern NSString *const CLCalendarDayTitleTextColor; //Day Title text color,  Mon, Tue, etc label text color

extern NSString *const CLCalendarSelectedDatePrintFormat;   //Selected Date print format,  - Default: @"EEE, d MMM yyyy"

extern NSString *const CLCalendarSelectedDatePrintColor;    //Selected Date print text color -Default: [UIColor whiteColor]

extern NSString *const CLCalendarSelectedDatePrintFontSize; //Selected Date print font size - Default : 13.f

extern NSString *const CLCalendarBackgroundImageColor;      //BackgroundImage color - Default : see applyCustomDefaults.
```


You need to fire below delegate function to apply your customisation
```objective-c
#pragma mark - CLWeeklyCalendarViewDelegate
-(NSDictionary *)CLCalendarBehaviorAttributes
{
    return @{
             CLCalendarWeekStartDay : @2, 	//Start from Tuesday every week
             CLCalendarDayTitleTextColor : [UIColor yellowColor],
             CLCalendarSelectedDatePrintColor : [UIColor greenColor],
             };
}


```



## Credits

CLFaceDetectionImagePicker is brought to you by [Caesar Li]

---
以上为原作者描述，为了适应自己的项目，我做了一些样式的修改：

1.改变日期字体大小
  调整"DailyCalendarView.m" 中  #define DATE_LABEL_FONT_SIZE 15
  
2.改变日期选中时圆圈的大小
 调整"DailyCalendarView.m" 中  #define DATE_LABEL_SIZE 22
 
3.改变日期颜色
在"DailyCalendarView.m" 中  -(void)markSelected:(BOOL)blnSelected;修改

     if([self.date isDateToday]){
        self.dateLabelContainer.backgroundColor = (blnSelected)?[UIColor colorWithHex:0xffc545]: [UIColor colorWithHex:0xfcdd9a];//设置今天日期的选中和非选中状态的圆圈背景色
        
        self.dateLabel.textColor = [UIColor whiteColor];//(blnSelected)?[UIColor colorWithHex:0x0081c1]:[UIColor whiteColor];设置今天日期的选中和非选中状态的文本颜色
    }else{
        self.dateLabelContainer.backgroundColor = (blnSelected)?[UIColor colorWithHex:0xffc545]: [UIColor clearColor];//设置选中非今天日期的选中和非选中状态的圆圈背景色
        
        self.dateLabel.textColor = [UIColor whiteColor];//(blnSelected)?[UIColor colorWithRed:52.0/255.0 green:161.0/255.0 blue:255.0/255.0 alpha:1.0]:[self colorByDate];//设置选中非今天日期的选中和非选中状态的文本颜色
    }
    
 4.星期开始日期设置
在`CLWeeklyCalendarViewDelegate`代理方法-(NSDictionary *)CLCalendarBehaviorAttributes中设置

    #pragma mark - CLWeeklyCalendarViewDelegate
    -(NSDictionary *)CLCalendarBehaviorAttributes
    {
    return @{
             CLCalendarWeekStartDay : @1,                 //设置星期开始日期，默认是星期一，1-7代表星期一到星期日           CLCalendarDayTitleTextColor : [UIColor yellowColor],//设置星期行文本颜色，默认#C2E8FF
    //             CLCalendarSelectedDatePrintColor : [UIColor greenColor],//设置第三行也就是显示当前选中日期行的文字颜色
           };
    }
    
 5.星期文本文字大小设置
 
调整`CLWeeklyCalendarView.m`中define DAY_TITLE_FONT_SIZE 12.f

6.星期文本文字样式

默认的星期样式是MON，我需要Mon的样式
修改`CLWeeklyCalendarView.m`中`-(UILabel *)dayTitleViewForDate: (NSDate *)date inFrame: (CGRect)frame`方法中 dayTitleLabel.text = [date getDayOfWeekShortString];//[[date getDayOfWeekShortString] uppercaseString];即可。
如果需要修改成中文样式，需要在`#import "NSDate+CL.h"`的方法`-(NSString *)getDayOfWeekShortString
`中修改NSLocale* en_AU_POSIX =[[NSLocale alloc] initWithLocaleIdentifier:@"en_AU_POSIX"];//@"zh_CN"表示中文
  