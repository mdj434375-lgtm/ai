<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>কার্টুন এআই বন্ধু</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #74ebd5 0%, #9face6 100%);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }

        /* কার্টুন রোবটের ডিজাইন */
        .robot-container {
            position: relative;
            margin-bottom: 30px;
        }

        .robot {
            width: 150px;
            height: 150px;
            background-color: #fff;
            border-radius: 50%;
            border: 8px solid #333;
            position: relative;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
            transition: all 0.3s ease;
        }

        /* অ্যান্টেনা */
        .antenna {
            width: 10px;
            height: 30px;
            background-color: #333;
            position: absolute;
            top: -35px;
            border-radius: 5px;
        }
        .antenna::after {
            content: '';
            width: 20px;
            height: 20px;
            background-color: #ff4757;
            border-radius: 50%;
            position: absolute;
            top: -15px;
            left: -5px;
        }

        /* চোখ */
        .eyes {
            display: flex;
            gap: 30px;
            margin-bottom: 15px;
        }
        .eye {
            width: 25px;
            height: 25px;
            background-color: #333;
            border-radius: 50%;
            /* চোখ পিটপিট করার অ্যানিমেশন */
            animation: blink 3s infinite; 
        }

        /* মুখ */
        .mouth {
            width: 60px;
            height: 10px;
            background-color: #333;
            border-radius: 20px;
            transition: all 0.1s ease;
        }

        /* কথা বলার সময় মুখ নড়াচড়ার অ্যানিমেশন */
        .talking .mouth {
            animation: talk 0.2s infinite alternate;
        }

        /* অ্যানিমেশন কী-ফ্রেম */
        @keyframes blink {
            0%, 96%, 100% { transform: scaleY(1); }
            98% { transform: scaleY(0.1); }
        }

        @keyframes talk {
            from { height: 10px; width: 60px; }
            to { height: 30px; width: 40px; border-radius: 50%; }
        }

        /* কথা বলার বাটন */
        button {
            padding: 15px 40px;
            font-size: 18px;
            font-weight: bold;
            background-color: #ff4757;
            color: white;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 5px 15px rgba(255, 71, 87, 0.4);
            transition: 0.3s;
        }
        button:hover {
            background-color: #ff6b81;
            transform: translateY(-2px);
        }
        
        #status {
            margin-top: 20px;
            font-size: 18px;
            color: #333;
            background: rgba(255, 255, 255, 0.8);
            padding: 10px 20px;
            border-radius: 10px;
            text-align: center;
            min-height: 25px;
            max-width: 80%;
        }
    </style>
</head>
<body>

    <div class="robot-container">
        <div class="antenna"></div>
        <div class="robot" id="robot">
            <div class="eyes">
                <div class="eye"></div>
                <div class="eye"></div>
            </div>
            <div class="mouth"></div>
        </div>
    </div>

    <button id="start-btn">🎤 কথা বলুন</button>
    <div id="status">হ্যালো! আমি আপনার কার্টুন বন্ধু। বাটনে ক্লিক করে কথা বলুন।</div>

    <script>
        const startBtn = document.getElementById('start-btn');
        const statusDiv = document.getElementById('status');
        const robot = document.getElementById('robot');

        const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        
        if (!SpeechRecognition) {
            statusDiv.innerText = "দুঃখিত, আপনার ব্রাউজার ভয়েস রেকগনিশন সাপোর্ট করে না। গুগল ক্রোম ব্যবহার করুন।";
        } else {
            const recognition = new SpeechRecognition();
            recognition.lang = 'bn-BD';
            recognition.interimResults = false;

            startBtn.onclick = () => {
                recognition.start();
                statusDiv.innerText = "🎙️ শুনছি... কথা বলুন।";
            };

            recognition.onresult = (event) => {
                const userSpeech = event.results[0][0].transcript;
                statusDiv.innerText = "আপনি বলেছেন: " + userSpeech;
                
                // এআই-এর উত্তর তৈরি
                setTimeout(() => {
                    let aiResponse = "আমি ঠিক বুঝতে পারিনি। আবার বলবেন?";
                    
                    if (userSpeech.includes("হ্যালো") || userSpeech.includes("হাই")) {
                        aiResponse = "হ্যালো! আমি আপনার কার্টুন বন্ধু। আপনার দিন কেমন যাচ্ছে?";
                    } else if (userSpeech.includes("কেমন আছো") || userSpeech.includes("কেমন আছেন")) {
                        aiResponse = "আমি দারুণ আছি! আপনি কেমন আছেন?";
                    } else if (userSpeech.includes("নাম কি")) {
                        aiResponse = "আমার নাম রোবো! আমি আপনার এআই বন্ধু।";
                    } else if (userSpeech.includes("গান")) {
                        aiResponse = "দুঃখিত, আমি এখনো গান গাইতে শিখিনি। তবে আমি কথা বলতে পারি!";
                    } else if (userSpeech.includes("সময়")) {
                        let time = new Date().toLocaleTimeString('bn-BD');
                        aiResponse = "এখন সময় " + time;
                    }

                    statusDiv.innerText = "রোবো বলছে: " + aiResponse;
                    speak(aiResponse);

                }, 1000);
            };

            // এআই-এর কথা বলা এবং ঠোঁট নড়ানো
            function speak(text) {
                const utterance = new SpeechSynthesisUtterance(text);
                utterance.lang = 'bn-BD'; 
                
                // কথা বলার সময় মুখ নড়ানোর জন্য ক্লাস অ্যাড করা
                robot.classList.add('talking');
                
                // কথা বলা শেষ হলে মুখ নড়ানো বন্ধ করা
                utterance.onend = () => {
                    robot.classList.remove('talking');
                    statusDiv.innerText = "আবার কথা বলতে বাটনে ক্লিক করুন।";
                };

                window.speechSynthesis.speak(utterance);
            }
        }
    </script>

</body>
</html>
