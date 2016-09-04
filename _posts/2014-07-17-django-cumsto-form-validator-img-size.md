---
layout: post
published: true
title: Django实践：自定义表单验证:词条唯一性,图片体积尺寸
---
以下代码用于 [liyuwiki.com](http://liyuwiki.com)  ，确保了用户在提交内容时，从form层面验证：词条是否已存在、图片文件是否符合要求。

```
def check_exist(value):
    t = Terms.objects.all()
    value = unicode(value)
    value = ' '.join(value.split())
    if t.filter(term=value).exists():
        uid = t.get(term=value).uid
        error_tips = mark_safe(u'【<a href="/%s.html">%s</a>】已经存在了' % (uid, value))
        raise ValidationError(error_tips)


def check_img(self):
    fileExtension = os.path.splitext(self._name)[1].lower()
    if fileExtension not in ['.jpg', '.jpeg', '.png', '.bmp', '.gif']:
        raise forms.ValidationError(u"文件类型错误： %s" % fileExtension)
    w, h = get_image_dimensions(self)
    if w > 1000:
        raise forms.ValidationError(u"你上传的图片宽度是 %ipx. 不能超过1000px." % w)
    if h > 3000:
        raise forms.ValidationError(u"你上传的图片高度是 %ipx. 不能超过3000px." % h)
```
以下是表单：
```
class TermForm(forms.Form):
    term = forms.CharField(max_length=20)
    docfile = forms.ImageField(required=False)
 
    def __init__(self, *args, **kwargs):
        super(TermForm, self).__init__(*args, **kwargs)
        self.fields['term'].validators.append(check_exist)
        self.fields['docfile'].validators.append(check_img)
    def clean_docfile(self):
        if self.cleaned_data['docfile']:
            if self.cleaned_data['docfile'].size > 3145728:
                raise forms.ValidationError(u"图片体积太大了.最多接受3MB.")
        return self.cleaned_data['docfile']
```

需要注意的是包含了html的ValidationError信息，使用了Django的mark_safe处理，以免被模板系统转义。
