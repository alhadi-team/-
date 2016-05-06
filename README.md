#  السلة الذهبية لتصنيف النفايات 
 يعتبر موضوع النفايات من المشكلات الأبرز التي تجتاح لبنان إذ أصبحنا نشاهد جبال من النفايات في شوارع المدينة ونشاهدكل يوم على التلفاز ضمن العنواين عن النفايات وتتصدر عنوين الصحف هذه المشكلة وقد تم تقديم العديد من الحلول إلا أن أفضل طريقة برئي الخبراء هو فرز النفايات من المصدر إلا أن مشكلة فرز النفايات تصادف الكثييير من المشاكل مثال حجم مستوعبات الفرز ,عدم توحيد الألوان , تعدد فتحات خيارات الرمي  يؤدي إإلى أخطاء لذلك تم إبتكار السلة الذهبية لتصنيف النفايات هي ,سلة شكلها دائري ,تفرز النفايات بواسطة الحساسات اللمس بعد تحديد النوع  و لها مدخل واحد,مساحتها صغيرة تستستخدم للمكفوفين ناطقة و الصم وعامة الناس


import RPi.GPIO as GPIO
import pygame
import time

btn1_pin = 22
btn2_pin = 17
btn3_pin = 4 
btn4_pin = 2

sw_pin = 23
relay_pin = 18


GPIO.setmode(GPIO.BCM)
GPIO.setup(btn1_pin, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(btn2_pin, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(btn3_pin, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(btn4_pin, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(btn5_pin, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(btn6_pin, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(sw_pin, GPIO.IN, pull_up_down=GPIO.PUD_UP)

GPIO.setup(relay_pin, GPIO.OUT)
GPIO.output(relay_pin, 0)



pygame.mixer.init(16000)


def motor(on):
	GPIO.output(relay_pin, on)

def input(i): 
	if (i == 1): return (int)(not GPIO.input(btn1_pin))
	elif (i == 2): return (int)(not GPIO.input(btn2_pin))
	elif (i == 3): return (int)(not GPIO.input(btn3_pin))
	elif (i == 4): return (int)(not GPIO.input(btn4_pin))
	elif (i == 5): return (int)(not GPIO.input(btn5_pin))
	elif (i == 6): return (int)(not GPIO.input(btn6_pin))
	elif (i == 7): return (int)(not GPIO.input(sw_pin))


def play(file):
	pygame.mixer.music.load(file)
	pygame.mixer.music.play()
	#while pygame.mixer.music.get_busy():
	#	continue


def turn(index):
	if (index == 1): play("carton.wav")
	elif (index == 2): play("plastic.wav")
	elif (index == 3): play("metal.wav")
	
	print "Turn: ", index


	for i in range(0, index):
		if (input(7)):
			print "w: !7..."
			motor(1)
			while (input(7)): continue
		time.sleep(0.3)
		print "w: 7... ", index
		motor(1)
		while (not input(7)): continue
		print "w7: ok"
	motor(0)

	print "w: 3000ms"
	time.sleep(3)

	for i in range(4 - index, 0, -1):
		if (input(7)):
			print "w: !7..."
			motor(1)
			while (input(7)): continue
		time.sleep(0.3)
		print "w: 7...", index
		motor(1)
		while (not input(7)): continue
		print "w7: ok"
	motor(0)
	
	print "done"
		

while 1:

	if (input(1)) : turn(1)
	elif (input(2)) : turn(2)
	elif (input(3)) : turn(3)


	#print input(1), input(2), input(3), input(4), input(5), input(6), input(7)
	motor(input(4))	




