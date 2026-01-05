<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>FET Final Vocabulary Challenge</title>
    <style>
        :root {
            --primary-color: #4a90e2;
            --primary-dark: #357abd;
            --secondary-color: #f0f2f5;
            --text-color: #2c3e50;
            --correct-bg: #d4edda;
            --correct-border: #c3e6cb;
            --correct-text: #155724;
            --wrong-bg: #f8d7da;
            --wrong-border: #f5c6cb;
            --wrong-text: #721c24;
            --card-shadow: 0 4px 12px rgba(0,0,0,0.08);
        }

        * {
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--secondary-color);
            color: var(--text-color);
            line-height: 1.5;
            margin: 0;
            padding: 15px;
            font-size: 16px; /* Base font size for mobile */
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: var(--card-shadow);
        }

        header {
            text-align: center;
            margin-bottom: 25px;
            border-bottom: 1px solid #eee;
            padding-bottom: 15px;
        }

        h1 {
            color: var(--primary-color);
            margin: 10px 0;
            font-size: 1.5rem;
        }

        .meta-info {
            color: #666;
            font-size: 0.9rem;
            margin-bottom: 15px;
        }

        .student-info {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 10px;
        }

        .student-info input {
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1rem;
            width: 100%;
            outline: none;
            transition: border-color 0.3s;
        }

        .student-info input:focus {
            border-color: var(--primary-color);
            box-shadow: 0 0 0 3px rgba(74, 144, 226, 0.2);
        }

        .question-block {
            margin-bottom: 25px;
            padding: 15px;
            border: 1px solid #ebedf0;
            border-radius: 10px;
            background-color: #fff;
        }

        .question-text {
            font-weight: 700;
            font-size: 1.1rem;
            margin-bottom: 15px;
            color: #2c3e50;
            line-height: 1.4;
        }

        .options {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .option-label {
            display: flex;
            align-items: flex-start; /* Align top for multi-line text */
            padding: 12px 15px;
            border: 2px solid #eef0f2;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s;
            position: relative;
            background-color: #fff;
        }

        .option-label:active {
            background-color: #f5f7fa;
            transform: scale(0.99);
        }

        /* Custom Radio Button Style */
        .option-label input[type="radio"] {
            margin-right: 12px;
            margin-top: 3px; /* Align with first line of text */
            min-width: 20px;
            min-height: 20px;
            cursor: pointer;
        }

        /* Selected State */
        .option-label:has(input:checked) {
            border-color: var(--primary-color);
            background-color: #f0f7ff;
        }

        /* Result Styles */
        .correct-answer {
            border-color: var(--correct-border) !important;
            background-color: var(--correct-bg) !important;
            color: var(--correct-text);
        }

        .correct-answer::after {
            content: "✓";
            position: absolute;
            right: 15px;
            font-weight: bold;
            font-size: 1.2rem;
        }

        .wrong-user-answer {
            border-color: var(--wrong-border) !important;
            background-color: var(--wrong-bg) !important;
            color: var(--wrong-text);
        }

        .wrong-user-answer::after {
            content: "✗";
            position: absolute;
            right: 15px;
            font-weight: bold;
            font-size: 1.2rem;
        }

        .feedback-text {
            margin-top: 12px;
            font-size: 0.95rem;
            font-weight: 600;
            display: none;
            padding: 8px;
            border-radius: 6px;
        }

        .submit-btn {
            display: block;
            width: 100%;
            padding: 16px;
            background-color: var(--primary-color);
            color: white;
            border: none;
            border-radius: 50px; /* Rounded pill shape */
            font-size: 1.1rem;
            font-weight: 700;
            cursor: pointer;
            margin-top: 30px;
            box-shadow: 0 4px 15px rgba(74, 144, 226, 0.4);
            transition: transform 0.2s, background-color 0.3s;
            -webkit-appearance: none; /* Remove default iOS style */
        }

        .submit-btn:active {
            transform: scale(0.98);
            background-color: var(--primary-dark);
        }

        .submit-btn:disabled {
            background-color: #ccc;
            cursor: not-allowed;
            box-shadow: none;
        }

        #result-area {
            display: none;
            text-align: center;
            margin-top: 30px;
            padding: 25px;
            background: linear-gradient(135deg, #2c3e50, #4ca1af);
            color: white;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }

        .score-circle {
            width: 120px;
            height: 120px;
            background: rgba(255,255,255,0.2);
            border-radius: 50%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            margin: 15px auto;
            border: 4px solid rgba(255,255,255,0.8);
        }

        .score-value {
            font-size: 2.5rem;
            font-weight: 800;
            line-height: 1;
        }
        
        .score-total {
            font-size: 0.9rem;
            opacity: 0.9;
        }

        .review-btn {
            padding: 12px 24px;
            background: white;
            color: #2c3e50;
            border: none;
            border-radius: 25px;
            font-weight: bold;
            margin-top: 15px;
            cursor: pointer;
            box-shadow: 0 2px 8px rgba(0,0,0,0.2);
        }

        /* Floating Progress Bar (Optional but nice) */
        .progress-bar {
            position: fixed;
            top: 0;
            left: 0;
            height: 4px;
            background: var(--primary-color);
            width: 0%;
            transition: width 0.3s;
            z-index: 1000;
        }

        @media (min-width: 600px) {
            .container {
                padding: 40px;
            }
            .student-info {
                flex-direction: row;
            }
            h1 {
                font-size: 2rem;
            }
            .option-label:hover {
                background-color: #f8f9fa;
                border-color: #d1d8e0;
            }
        }
    </style>
</head>
<body>

<div class="progress-bar" id="progressBar"></div>

<div class="container">
    <header>
        <h1>FET Final Vocabulary Challenge</h1>
        <div class="meta-info">50 Questions • 2 Points Each • Total 100 Points</div>
        <div class="student-info">
            <input type="text" id="studentName" placeholder="Enter Your Name">
            <input type="text" id="quizDate" placeholder="Date" onfocus="(this.type='date')">
        </div>
    </header>

    <form id="quizForm">
        <div id="questions-container">
            <!-- Questions will be injected here by JavaScript -->
        </div>

        <button type="button" class="submit-btn" onclick="submitQuiz()">Submit Quiz</button>
    </form>

    <div id="result-area">
        <h2 style="margin:0">Quiz Results</h2>
        <div id="student-display" style="margin-top:5px; opacity:0.9"></div>
        
        <div class="score-circle">
            <div class="score-value" id="score-value">0</div>
            <div class="score-total">/ 100</div>
        </div>
        
        <p id="message" style="font-weight:bold; font-size:1.1rem; margin:10px 0;"></p>
        <button class="review-btn" onclick="reviewQuiz()">Review Answers</button>
    </div>
</div>

<script>
    const questions = [
        {
            q: "1. What is the best definition of 'emergency'?",
            options: [
                "(A) A planned event that occurs regularly without any surprise",
                "(B) A serious, unexpected situation that requires immediate action",
                "(C) A dangerous weapon used to protect people during a war",
                "(D) A long journey taken to a distant and unknown country"
            ],
            answer: 1 // B
        },
        {
            q: "2. Which word means 'to bring your foot down heavily and noisily'?",
            options: [
                "(A) stumble",
                "(B) slide",
                "(C) sprint",
                "(D) stomp"
            ],
            answer: 3 // D
        },
        {
            q: "3. What does it mean to be 'eager'?",
            options: [
                "(A) Strong wanting to do or have something soon",
                "(B) Feeling very tired and needing rest immediately",
                "(C) Being afraid of what might happen in the future",
                "(D) Having a lot of knowledge about a specific topic"
            ],
            answer: 0 // A
        },
        {
            q: "4. A 'myth' is best defined as:",
            options: [
                "(A) A scientific fact proved by experiments",
                "(B) A written record of daily personal events",
                "(C) A funny joke told to entertain a large audience",
                "(D) A traditional story explaining natural or social history"
            ],
            answer: 3 // D
        },
        {
            q: "5. What is the definition of 'crisis'?",
            options: [
                "(A) A period of calm and peaceful relaxation",
                "(B) A time of intense difficulty, trouble, or danger",
                "(C) A solution to a very simple mathematical problem",
                "(D) A large celebration involving many people"
            ],
            answer: 1 // B
        },
        {
            q: "6. Which word means 'to copy the speech or behavior of someone'?",
            options: [
                "(A) ignore",
                "(B) invent",
                "(C) imitate",
                "(D) inspire"
            ],
            answer: 2 // C
        },
        {
            q: "7. What is a 'refugee'?",
            options: [
                "(A) A person who travels to different countries for a holiday",
                "(B) A person forced to leave their country to escape war",
                "(C) A person who moves to a new city to find a better job",
                "(D) A person sent to prison for breaking the law"
            ],
            answer: 1 // B
        },
        {
            q: "8. 'Debris' refers to:",
            options: [
                "(A) Valuable items hidden underground for safety",
                "(B) A collection of new tools used for construction",
                "(C) The fresh soil used for planting a garden",
                "(D) Scattered pieces of waste or remains after destruction"
            ],
            answer: 3 // D
        },
        {
            q: "9. If something is 'sufficient', it is:",
            options: [
                "(A) Enough or adequate",
                "(B) Too much to handle",
                "(C) Empty or hollow",
                "(D) Lacking in quality"
            ],
            answer: 0 // A
        },
        {
            q: "10. What is a 'prosthetic'?",
            options: [
                "(A) A protective gear worn by athletes during a game",
                "(B) An artificial body part used to replace a missing one",
                "(C) A natural bone structure found inside the body",
                "(D) A medical tool used to measure blood pressure"
            ],
            answer: 1 // B
        },
        {
            q: "11. A 'poacher' is someone who:",
            options: [
                "(A) Protects wild animals from danger",
                "(B) Studies animals in their natural habitat",
                "(C) Raises animals on a large farm",
                "(D) Hunts or catches animals illegally"
            ],
            answer: 3 // D
        },
        {
            q: "12. What is the purpose of a 'vaccine'?",
            options: [
                "(A) To cure a broken bone or injury immediately",
                "(B) To measure the temperature of a sick patient",
                "(C) To stimulate antibodies and provide immunity against disease",
                "(D) To provide nutrition and energy for the body"
            ],
            answer: 2 // C
        },
        {
            q: "13. To 'motivate' means to:",
            options: [
                "(A) Force someone to do something against their will",
                "(B) Provide someone with a reason or encouragement to act",
                "(C) Stop someone from making a big mistake",
                "(D) Watch someone carefully without saying anything"
            ],
            answer: 1 // B
        },
        {
            q: "14. A 'victim' is:",
            options: [
                "(A) A person harmed or injured as a result of a crime",
                "(B) A person who commits a crime against others",
                "(C) A person who witnesses an accident happen",
                "(D) A person who helps save others from danger"
            ],
            answer: 0 // A
        },
        {
            q: "15. Which word is a synonym for 'courageous'?",
            options: [
                "(A) dangerous",
                "(B) nervous",
                "(C) cautious",
                "(D) brave"
            ],
            answer: 3 // D
        },
        {
            q: "16. Something described as 'giant' is:",
            options: [
                "(A) Extremely small and hard to see",
                "(B) Average in height and weight",
                "(C) Of very great size or force; huge",
                "(D) Broken into many tiny pieces"
            ],
            answer: 2 // C
        },
        {
            q: "17. A 'herd' refers to:",
            options: [
                "(A) A large group of birds flying together",
                "(B) A group of animals feeding or moving together",
                "(C) A pack of wolves hunting in the forest",
                "(D) A school of fish swimming in the ocean"
            ],
            answer: 1 // B
        },
        {
            q: "18. 'Teamwork' is best defined as:",
            options: [
                "(A) The combined action of a group working effectively",
                "(B) The effort of a single person working alone",
                "(C) A competition where people fight against each other",
                "(D) A leadership role taken by one boss"
            ],
            answer: 0 // A
        },
        {
            q: "19. A 'volunteer' is someone who:",
            options: [
                "(A) Works hard in exchange for a high salary",
                "(B) Is forced to work by the government",
                "(C) Manages a company and hires employees",
                "(D) Freely offers to do something without being paid"
            ],
            answer: 3 // D
        },
        {
            q: "20. To 'fasten' means to:",
            options: [
                "(A) Close or join something securely",
                "(B) Move at a very high speed",
                "(C) Eat very little food for a day",
                "(D) Break something into two parts"
            ],
            answer: 0 // A
        },
        {
            q: "21. What is a 'firefly'?",
            options: [
                "(A) A small spark created by a campfire",
                "(B) A dangerous fly that bites humans",
                "(C) A soft-bodied beetle that glows at night",
                "(D) A mythical creature that breathes fire"
            ],
            answer: 2 // C
        },
        {
            q: "22. A 'device' is:",
            options: [
                "(A) A detailed plan for a future project",
                "(B) A tool or gadget made for a particular purpose",
                "(C) A piece of advice given by a teacher",
                "(D) A natural object found in the forest"
            ],
            answer: 1 // B
        },
        {
            q: "23. A 'solution' is:",
            options: [
                "(A) A difficult puzzle that takes time to finish",
                "(B) A means of solving a problem or dealing with a situation",
                "(C) A mistake made during a calculation",
                "(D) A question that has no clear answer"
            ],
            answer: 1 // B
        },
        {
            q: "24. Which word means 'to care for or look after'?",
            options: [
                "(A) tend",
                "(B) mend",
                "(C) send",
                "(D) bend"
            ],
            answer: 0 // A
        },
        {
            q: "25. If you are 'shocked', you are:",
            options: [
                "(A) Feeling extremely tired and sleepy",
                "(B) Very happy about a piece of good news",
                "(C) Surprised or upset by something unexpected",
                "(D) Angry at someone for making a mistake"
            ],
            answer: 2 // C
        },
        {
            q: "26. A 'skill' is:",
            options: [
                "(A) A lucky event that happens by chance",
                "(B) A feeling of fear when facing danger",
                "(C) A tool used to fix broken machines",
                "(D) The ability to do something well; expertise"
            ],
            answer: 3 // D
        },
        {
            q: "27. Someone who is 'clever' is:",
            options: [
                "(A) Slow to understand new ideas",
                "(B) Quick to learn and apply ideas; smart",
                "(C) Very strong but not very intelligent",
                "(D) Always angry and difficult to talk to"
            ],
            answer: 1 // B
        },
        {
            q: "28. 'Electricity' is:",
            options: [
                "(A) A form of energy resulting from charged particles",
                "(B) A type of fuel used to power old trains",
                "(C) A natural gas found deep underground",
                "(D) A liquid used to cool down machines"
            ],
            answer: 0 // A
        },
        {
            q: "29. A 'pasture' is:",
            options: [
                "(A) A large desert with plenty of sand",
                "(B) Land covered with grass for grazing animals",
                "(C) A deep forest with many tall trees",
                "(D) A high mountain covered in snow"
            ],
            answer: 1 // B
        },
        {
            q: "30. To 'intervene' means to:",
            options: [
                "(A) Watch a situation without doing anything",
                "(B) Cause a fight between two people",
                "(C) Come between to prevent or alter a result",
                "(D) Leave a place immediately"
            ],
            answer: 2 // C
        },
        {
            q: "31. What is 'punishment'?",
            options: [
                "(A) A penalty inflicted as retribution for an offense",
                "(B) A reward given for excellent performance",
                "(C) A game played by children in school",
                "(D) A gift given on a special occasion"
            ],
            answer: 0 // A
        },
        {
            q: "32. To 'survive' means to:",
            options: [
                "(A) Give up when things get difficult",
                "(B) Continue to live or exist, especially in spite of danger",
                "(C) Disappear completely without a trace",
                "(D) Suffers from a long-term illness"
            ],
            answer: 1 // B
        },
        {
            q: "33. What does 'scatter' mean?",
            options: [
                "(A) To gather items into a neat pile",
                "(B) To glue two pieces of paper together",
                "(C) To throw in various random directions",
                "(D) To build a structure using bricks"
            ],
            answer: 2 // C
        },
        {
            q: "34. To 'infer' is to:",
            options: [
                "(A) State a fact clearly and loudly",
                "(B) Ignore the details of a story",
                "(C) Create a new story from imagination",
                "(D) Deduce or conclude information from evidence"
            ],
            answer: 3 // D
        },
        {
            q: "35. To be 'aware' means:",
            options: [
                "(A) To be asleep and dreaming",
                "(B) To have knowledge or perception of a situation",
                "(C) To be confused about what is happening",
                "(D) To be lost in a strange place"
            ],
            answer: 1 // B
        },
        {
            q: "36. Which word is a synonym for 'aid'?",
            options: [
                "(A) harm",
                "(B) block",
                "(C) help",
                "(D) stop"
            ],
            answer: 2 // C
        },
        {
            q: "37. To 'attach' means to:",
            options: [
                "(A) Join or fasten one thing to another",
                "(B) Separate two things that are glued",
                "(C) Break something into small pieces",
                "(D) Lose something important"
            ],
            answer: 0 // A
        },
        {
            q: "38. To 'decide' is to:",
            options: [
                "(A) Guess the answer without thinking",
                "(B) Come to a resolution after consideration",
                "(C) Forget what you were going to do",
                "(D) Wait for someone else to choose"
            ],
            answer: 1 // B
        },
        {
            q: "39. To 'commit' means to:",
            options: [
                "(A) Run away from a difficult problem",
                "(B) Make a joke to make people laugh",
                "(C) Pledge or bind oneself to a certain course",
                "(D) Sleep for a very long time"
            ],
            answer: 2 // C
        },
        {
            q: "40. If two things are 'similar', they are:",
            options: [
                "(A) Exactly the same in every way",
                "(B) Completely different from each other",
                "(C) Opposite in meaning or appearance",
                "(D) Alike but not identical"
            ],
            answer: 3 // D
        },
        {
            q: "41. What does 'secure' mean?",
            options: [
                "(A) Loose and likely to fall off",
                "(B) Fixed or fastened so it will not be lost",
                "(C) Dangerous and risky to use",
                "(D) Scary and frightening to look at"
            ],
            answer: 1 // B
        },
        {
            q: "42. To 'invent' means to:",
            options: [
                "(A) Find something that was lost",
                "(B) Create or design something that has not existed before",
                "(C) Destroy something that is old",
                "(D) Buy something from a store"
            ],
            answer: 1 // B
        },
        {
            q: "43. Something 'effective' is:",
            options: [
                "(A) Successful in producing a desired result",
                "(B) Useless and does not work at all",
                "(C) Broken and needs to be fixed",
                "(D) Cheap and easy to break"
            ],
            answer: 0 // A
        },
        {
            q: "44. To 'graze' means to:",
            options: [
                "(A) Sleep in a warm place",
                "(B) Eat grass in a field",
                "(C) Run very fast in a race",
                "(D) Swim across a river"
            ],
            answer: 1 // B
        },
        {
            q: "45. To 'protect' means to:",
            options: [
                "(A) Attack someone suddenly",
                "(B) Keep safe from harm or injury",
                "(C) Ignore a warning sign",
                "(D) Lose a valuable item"
            ],
            answer: 1 // B
        },
        {
            q: "46. A 'border' is:",
            options: [
                "(A) The center point of a city",
                "(B) A line separating two geographical areas",
                "(C) A deep hole in the ground",
                "(D) A mountain that is hard to climb"
            ],
            answer: 1 // B
        },
        {
            q: "47. 'Local' means:",
            options: [
                "(A) Relating to a particular area or neighborhood",
                "(B) Coming from a very distant country",
                "(C) Relating to the entire world",
                "(D) Existing only in the past"
            ],
            answer: 0 // A
        },
        {
            q: "48. An 'origin' is:",
            options: [
                "(A) The final result of a process",
                "(B) The point or place where something begins",
                "(C) The middle part of a journey",
                "(D) The reason why something failed"
            ],
            answer: 1 // B
        },
        {
            q: "49. To 'weave' means to:",
            options: [
                "(A) Form fabric by interlacing threads",
                "(B) Paint a picture using bright colors",
                "(C) Carve a statue out of stone",
                "(D) Melt metal to make a tool"
            ],
            answer: 0 // A
        },
        {
            q: "50. To 'get along' means to:",
            options: [
                "(A) Argue and fight constantly",
                "(B) Run away to a different place",
                "(C) Have a harmonious or friendly relationship",
                "(D) Be alone and avoid people"
            ],
            answer: 2 // C
        }
    ];

    function loadQuiz() {
        const container = document.getElementById('questions-container');
        questions.forEach((q, index) => {
            const block = document.createElement('div');
            block.className = 'question-block';
            block.id = `q${index}`;

            let optionsHtml = '';
            q.options.forEach((opt, optIndex) => {
                optionsHtml += `
                    <label class="option-label" id="label-q${index}-o${optIndex}">
                        <input type="radio" name="q${index}" value="${optIndex}" onchange="updateProgress()">
                        <div>${opt}</div>
                    </label>
                `;
            });

            block.innerHTML = `
                <div class="question-text">${q.q}</div>
                <div class="options">
                    ${optionsHtml}
                </div>
                <div class="feedback-text" id="feedback-q${index}"></div>
            `;
            container.appendChild(block);
        });
    }

    function updateProgress() {
        const total = questions.length;
        const answered = document.querySelectorAll('input[type="radio"]:checked').length;
        const percent = (answered / total) * 100;
        document.getElementById('progressBar').style.width = percent + '%';
    }

    function submitQuiz() {
        let score = 0;
        let answeredCount = 0;
        
        // Disable button
        const submitBtn = document.querySelector('.submit-btn');
        submitBtn.disabled = true;
        submitBtn.innerText = 'Grading...';
        
        // Fill progress bar to 100% on submit
        document.getElementById('progressBar').style.width = '100%';

        questions.forEach((q, index) => {
            const selected = document.querySelector(`input[name="q${index}"]:checked`);
            const feedback = document.getElementById(`feedback-q${index}`);
            
            // Mark the correct answer green
            const correctLabel = document.getElementById(`label-q${index}-o${q.answer}`);
            correctLabel.classList.add('correct-answer');

            if (selected) {
                answeredCount++;
                const userVal = parseInt(selected.value);
                if (userVal === q.answer) {
                    score += 2;
                    feedback.style.display = 'block';
                    feedback.style.backgroundColor = '#d4edda'; // Light green bg
                    feedback.style.color = '#155724';
                    feedback.innerText = '✓ Correct';
                } else {
                    // Mark wrong answer red
                    const wrongLabel = document.getElementById(`label-q${index}-o${userVal}`);
                    wrongLabel.classList.add('wrong-user-answer');
                    feedback.style.display = 'block';
                    feedback.style.backgroundColor = '#f8d7da'; // Light red bg
                    feedback.style.color = '#721c24';
                    feedback.innerText = '✗ Incorrect';
                }
            } else {
                 feedback.style.display = 'block';
                 feedback.style.backgroundColor = '#fff3cd'; // Light yellow bg
                 feedback.style.color = '#856404';
                 feedback.innerText = '⚠ Not Answered';
            }
        });

        // Show results
        const name = document.getElementById('studentName').value || 'Student';
        document.getElementById('student-display').innerHTML = `Great job, ${name}!`;
        
        // Animate Score
        animateValue("score-value", 0, score, 1500);
        
        const message = document.getElementById('message');
        if (score === 100) message.innerText = "Perfect Score! Amazing! 🎉";
        else if (score >= 80) message.innerText = "Excellent work! Keep it up! 🌟";
        else if (score >= 60) message.innerText = "Good job! Review the ones you missed. 👍";
        else message.innerText = "Keep practicing! You'll get better. 💪";

        document.getElementById('result-area').style.display = 'block';
        
        // Scroll to results
        setTimeout(() => {
            document.getElementById('result-area').scrollIntoView({ behavior: 'smooth' });
        }, 100);
    }

    function animateValue(id, start, end, duration) {
        if (start === end) return;
        const range = end - start;
        let current = start;
        const increment = end > start ? 1 : -1;
        const stepTime = Math.abs(Math.floor(duration / range));
        const obj = document.getElementById(id);
        
        const timer = setInterval(function() {
            current += increment;
            obj.innerHTML = current;
            if (current == end) {
                clearInterval(timer);
            }
        }, stepTime);
    }

    function reviewQuiz() {
        window.scrollTo({top: 0, behavior: 'smooth'});
    }

    // Initialize
    window.onload = loadQuiz;

</script>

</body>
</html>
