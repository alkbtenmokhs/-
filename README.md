# ----

from PIL import Image, ImageDraw, ImageFont, ImageFilter

# إعداد حجم الصورة وخلفيتها
img_width, img_height = 800, 400
background_color = (0, 0, 0)  # خلفية سوداء
image = Image.new("RGB", (img_width, img_height), background_color)
draw = ImageDraw.Draw(image)

# تحميل الخطوط (تأكد من توفر ملفات الخطوط المناسبة)
# قم بتعديل مسارات الخطوط حسب ما هو متوفر لديك
try:
    font_ar = ImageFont.truetype("arial.ttf", 70)  # مثال لخط يدعم العربية
    font_en = ImageFont.truetype("arial.ttf", 50)
except IOError:
    # في حال لم تجد الخط المطلوب، سيتم استخدام الخط الافتراضي
    font_ar = ImageFont.load_default()
    font_en = ImageFont.load_default()

# إعداد النصوص
text_ar = "المتحدة تال"
text_en = "UNITED TAL"

# حساب أبعاد النصوص لمركزتها
w_ar, h_ar = draw.textsize(text_ar, font=font_ar)
w_en, h_en = draw.textsize(text_en, font=font_en)

# تحديد مواضع النصوص (مثال: النص العربي في الأعلى والنص الإنجليزي في الأسفل)
x_ar = (img_width - w_ar) // 2
y_ar = (img_height // 2) - h_ar - 10
x_en = (img_width - w_en) // 2
y_en = (img_height // 2) + 10

# رسم تأثير الظل (drop shadow) لإضفاء مظهر بارز للنصوص
shadow_color = (50, 50, 50)  # لون الظل
offset = 3  # إزاحة الظل

# النص العربي مع ظل
draw.text((x_ar + offset, y_ar + offset), text_ar, font=font_ar, fill=shadow_color)
# النص الإنجليزي مع ظل
draw.text((x_en + offset, y_en + offset), text_en, font=font_en, fill=shadow_color)

# رسم النصوص بلون أبيض لإبرازها
text_color = (255, 255, 255)  # لون النص الرئيسي

draw.text((x_ar, y_ar), text_ar, font=font_ar, fill=text_color)
draw.text((x_en, y_en), text_en, font=font_en, fill=text_color)

# لإضافة تأثير يشبه "الرذاذ" يمكن تطبيق غموض خفيف (blur) مع طبقات متعددة
# هنا نقوم بنسخ الصورة ثم دمجها لإعطاء بعض الحيوية للتأثير
spray = image.filter(ImageFilter.GaussianBlur(2))
image = Image.blend(image, spray, alpha=0.25)

# حفظ الصورة النتيجة
image.save("design_united_tal.png")
image.show()
