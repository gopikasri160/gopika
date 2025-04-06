<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vignan's Institute of Information Technology - Admission Chatbot</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }
        body {
            background-image: url('https://leavesinternational.com/wp-content/uploads/2023/02/Vignan_logo.png');
            background-size: cover;
            background-position: center;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }
        .chat-container {
            width: 100%;
            height: 100%;
            max-width: 600px;
            max-height: 800px;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
            display: flex;
            flex-direction: column;
        }
        .chat-box {
            padding: 20px;
            flex: 1;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            height: 70%; /* Increased height for chat box */
        }
        .chat-message {
            background: #f1f1f1;
            padding: 10px;
            border-radius: 10px;
            margin: 5px 0;
            max-width: 80%;
            font-size: 18px; /* Increased font size */
        }
        .user-message {
            align-self: flex-end; /* Align user messages to the right */
            background: #87CEEB;
            color: black;
        }
        .bot-message {
            align-self: flex-start; /* Align bot messages to the left */
            background: #000080;
            color: white;
            font-size: 18px; /* Increased font size */
        }
        .chat-input {
            display: flex;
            padding: 10px;
            background: white;
            border-top: 1px solid #ccc;
        }
        .chat-input input {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 5px;
            outline: none;
        }
        .chat-input button {
            margin-left: 10px;
            padding: 10px 15px;
            background: blue;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        h1 {
            text-align: center;
            font-size: 32px;
            margin-bottom: 10px;
            color: darkviolet;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
            font-weight: bold;
        }
        .options {
            margin: 5px 20px;
        }
        .options label {
            display: block;
            margin: 5px 0;
            position: relative;
            padding-left: 30px; /* Space for custom radio button */
            cursor: pointer;
            font-size: 18px; /* Increased font size */
        }
        .options input[type="radio"] {
            position: absolute;
            opacity: 0; /* Hide the default radio button */
            cursor: pointer;
        }
        .options .custom-radio {
            position: absolute;
            top: 50%;
            left: 0;
            transform: translateY(-50%);
            width: 20px;
            height: 20px;
            border-radius: 50%;
            border: 2px solid #000;
            background-color: transparent;
        }
        .options input[type="radio"]:checked + .custom-radio {
            background-color: #87CEEB; /* Color when checked */
            border-color: #000080; /* Border color when checked */
        }
        table {
            border-collapse: collapse;
            width: 100%;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #f0f0f0;
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <h1>Vignan's Institute of Information Technology - Admission Chatbot</h1>
        <div class="chat-box" id="chat-box">
            <div class="chat-message bot-message">Hello! Please select a branch or an option below to get started.</div>
        </div>
        <div class="options" id="options-container"></div>
        <div class="chat-input">
            <button onclick="sendMessage()" class="send-btn">Send</button>
        </div>
    </div>
    
    <script>
        const responses = {
            "hi": "Welcome to Vignan's Institute of Information Technology!",
            "admission process": "Admissions are done through convener quota based on AP EAPCET rank.",
            "convener quota": "Seats are allotted through the AP EAPCET counseling process.",
            "scholarship quota": "Scholarships are available based on merit and category.",
            "documents required": "Required Documents: 10th & Inter certificates, Rank card, TC, Caste & Income certificates, Aadhar, etc.",
            "fee structure": "The first-year fee is ₹68,700.",
            "important dates": {
                "Application Start Date": "April 1, 2024",
                "Last Date to Apply": "May 31, 2024",
                "AP EAPCET Registration Timeline": "Mar 12, 2025 - Apr 15, 2025",
                "AP EAPCET ADMIT CARD DATE": "May 07, 2025",
                "AP EAPCET Exam Date": "May 16, 2025 - May 23, 2025",
                "AP EAPCET Result Date": "Jun 14, 2025",
                "AP EAPCET Counselling Date": "Jul 01, 2025 - Jul 07, 2025"
            },
            "hod of cse": "HOD of CSE: Dr. DINESH REDDY.",
            "hod of ece": "HOD of ECE: Dr. RAMESH BABU.",
            "hod of ai & ds": "HOD of AI & DS: Dr. T.V Madhusudhana Rao.",
            "hod of mec": "HOD of MEC: Mr. Rambabu Sarimalla.",
            "hod of eee": "HOD of EEE: Dr. PUDI SEKHAR.",
            "hod of civ": "HOD of CIV: Dr. GOVINDA LALAM.",
            "hod of inf": "HOD of INF: Dr. G. NEELIMA.",
            "hod of cai": "HOD of CAI: Dr. K. SWATHI.",
            "hod of csc": "HOD of CSC: Dr. K. SWATHI.",
            "hod of csd": "HOD of CSD: Dr. K. SWATHI."
        };

        const branches = [
            "CSE", "ECE", "AI & DS", "MEC", "EEE", "CIV", "INF", "CAI", "CSC", "CSD"
        ];

        const cutoffs = {
            "CSE": {
                "OC": { "Boys": 13745, "Girls": 12108 },
                "EWS": { "Boys": 25234, "Girls": 25518 },
                "BC-A": { "Boys": 22531, "Girls": 19841 },
                "BC-B": { "Boys": 29577, "Girls": 29956 },
                "BC-C": { "Boys": 15081, "Girls": 31226 },
                "BC-D": { "Boys": 17606, "Girls": 16870 },
                "SC": { "Boys": 52983, "Girls": 57037 },
                "ST": { "Boys": 51726, "Girls": 87060 }
            },
            "ECE": {
                "OC": { "Boys": 21738, "Girls": 26547 },
                "EWS": { "Boys": 45912, "Girls": 40060 },
                "BC-A": { "Boys": 35903, "Girls": 34771 },
                "BC-B": { "Boys": 39870, "Girls": 38304 },
                "BC-C": { "Boys": 28931, "Girls": 28569 },
                "BC-D": { "Boys": 79581, "Girls": 58614 },
                "SC": { "Boys": 121200, "Girls": 116032 },
                "ST": { "Boys": 131052, "Girls": 140795 }
            },
            "AI & DS": {
                "OC": { "Boys": 16649, "Girls": 16265 },
                "EWS": { "Boys": 33761, "Girls": 27973 },
                "BC-A": { "Boys": 33444, "Girls": 47337 },
                "BC-B": { "Boys": 58225, "Girls": 57784 },
                "SC": { "Boys": 125768, "Girls": 130344 },
                "ST": { "Boys": 142777, "Girls": 103194 }
            },
            "MEC": {
                "OC": { "Boys": 43513, "Girls": 79761 },
                "EWS": { "Boys": 129810, "Girls": 120931 },
                "BC-A": { "Boys": 112417, "Girls": 170354 },
                "BC-B": { "Boys": 133363, "Girls": 124589 },
                "SC": { "Boys": 174593, "Girls": 162121 },
                "ST": { "Boys": 159584, "Girls": 169614 }
            },
            "EEE": {
                "OC": { "Boys": 38517, "Girls": 52386 },
                "EWS": { "Boys": 77155, "Girls": 92347 },
                "BC-A": { "Boys": 75432, "Girls": 112127 },
                "BC-B": { "Boys": 81382, "Girls": 86127 },
                "SC": { "Boys": 147757, "Girls": 142592 },
                "ST": { "Boys": 159584, "Girls": 169614 }
            },
            "CIV": {
                "OC": { "Boys": 71260, "Girls": 61598 },
                "EWS": { "Boys": 131052, "Girls": 140795 },
                "BC-A": { "Boys": 121961, "Girls": 120345 },
                "BC-B": { "Boys": 162840, "Girls": 175856 },
                "SC": { "Boys": 159584, "Girls": 169614 },
                "ST": { "Boys": 159584, "Girls": 169614 }
            },
            "INF": {
                "OC": { "Boys": 19149, "Girls": 26994 },
                "EWS": { "Boys": 48461, "Girls": 37782 },
                "BC-A": { "Boys": 37447, "Girls": 47979 },
                "BC-B": { "Boys": 58035, "Girls": 57703 },
                "SC": { "Boys": 157459, "Girls": 117939 },
                "ST": { "Boys": 159584, "Girls": 169614 }
            },
            "CAI": {
                "OC": { "Boys": 16075, "Girls": 13385 },
                "EWS": { "Boys": 35430, "Girls": 23480 },
                "BC-A": { "Boys": 28429, "Girls": 41629 },
                "BC-B": { "Boys": 30435, "Girls": 48154 },
                "SC": { "Boys": 142777, "Girls": 103194 },
                "ST": { "Boys": 159584, "Girls": 169614 }
            },
            "CSC": {
                "OC": { "Boys": 17294, "Girls": 12154 },
                "EWS": { "Boys": 32646, "Girls": 30657 },
                "BC-A": { "Boys": 33349, "Girls": 36464 },
                "BC-B": { "Boys": 42187, "Girls": 39230 },
                "SC": { "Boys": 161278, "Girls": 115637 },
                "ST": { "Boys": 159584, "Girls": 169614 }
            },
            "CSD": {
                "OC": { "Boys": 18888, "Girls": 16772 },
                "EWS": { "Boys": 32850, "Girls": 32262 },
                "BC-A": { "Boys": 39938, "Girls": 30720 },
                "BC-B": { "Boys": 39003, "Girls": 30319 },
                "SC": { "Boys": 98315, "Girls": 53719 },
                "ST": { "Boys": 159584, "Girls": 169614 }
            }
        };

        let currentStep = 0;
        let selectedBranch = '';
        let selectedCategory = '';
        let selectedGender = '';

        function sendMessage() {
            const chatBox = document.getElementById('chat-box');
            const userInput = document.querySelector('input[name="branch"]:checked') || document.querySelector('input[name="option"]:checked');
            const botMessage = document.createElement('div');

            if (currentStep === 0) {
                if (userInput) {
                    if (userInput.name === "branch") {
                        selectedBranch = userInput.value;

                        // User message
                        const userMessage = document.createElement('div');
                        userMessage.classList.add('chat-message', 'user-message');
                        userMessage.innerText = You selected ${selectedBranch}.;
                        chatBox.appendChild(userMessage);

                        // Bot message
                        botMessage.classList.add('chat-message', 'bot-message');
                        botMessage.innerText = Would you like to see the cutoffs or the HOD for ${selectedBranch}?;
                        chatBox.appendChild(botMessage);
                        chatBox.scrollTop = chatBox.scrollHeight;

                        currentStep = 1;
                        renderOptions(["cutoffs", "hod"]);
                    } else {
                        if (userInput.value === "documents required") {
                            botMessage.classList.add('chat-message', 'bot-message');
                            botMessage.innerText = responses["documents required"];
                            chatBox.appendChild(botMessage);
                            chatBox.scrollTop = chatBox.scrollHeight;
                            currentStep = 0;
                            renderOptions(branches);
                        } else if (userInput.value === "fee structure") {
                            botMessage.classList.add('chat-message', 'bot-message');
                            botMessage.innerText = responses["fee structure"];
                            chatBox.appendChild(botMessage);
                            chatBox.scrollTop = chatBox.scrollHeight;
                            currentStep = 0;
                            renderOptions(branches);
                        } else if (userInput.value === "important dates") {
                            const table = document.createElement('table');
                            const thead = document.createElement('thead');
                            const tbody = document.createElement('tbody');
                            table.appendChild(thead);
                            table.appendChild(tbody);
                            const tr = document.createElement('tr');
                            thead.appendChild(tr);
                            const th1 = document.createElement('th');
                            th1.innerText = "Event";
                            tr.appendChild(th1);
                            const th2 = document.createElement('th');
                            th2.innerText = "Date";
                            tr.appendChild(th2);
                            for (const [event, date] of Object.entries(responses["important dates"])) {
                                const tr = document.createElement('tr');
                                tbody.appendChild(tr);
                                const td1 = document.createElement('td');
                                td1.innerText = event;
                                tr.appendChild(td1);
                                const td2 = document.createElement('td');
                                td2.innerText = date;
                                tr.appendChild(td2);
                            }
                            botMessage.classList.add('chat-message', 'bot-message');
                            botMessage.appendChild(table);
                            chatBox.appendChild(botMessage);
                            chatBox.scrollTop = chatBox.scrollHeight;
                            currentStep = 0;
                            renderOptions(branches);
                        }
                    }
                } else {
                    botMessage.innerText = "Please select a branch or an option.";
                    chatBox.appendChild(botMessage);
                }
            } else if (currentStep === 1) {
                if (userInput) {
                    if (userInput.value === "cutoffs") {
                        // User message
                        const userMessage = document.createElement('div');
                        userMessage.classList.add('chat-message', 'user-message');
                        userMessage.innerText = You selected cutoffs for ${selectedBranch}.;
                        chatBox.appendChild(userMessage);

                        // Bot message
                        botMessage.classList.add('chat-message', 'bot-message');
                        botMessage.innerText = Please select a category.;
                        chatBox.appendChild(botMessage);
                        chatBox.scrollTop = chatBox.scrollHeight;

                        currentStep = 2;
                        const categories = Object.keys(cutoffs[selectedBranch]);
                        renderOptions(categories);
                    } else if (userInput.value === "hod") {
                        // User message
                        const userMessage = document.createElement('div');
                        userMessage.classList.add('chat-message', 'user-message');
                        userMessage.innerText = You selected HOD for ${selectedBranch}.;
                        chatBox.appendChild(userMessage);

                        // Bot message
                        botMessage.classList.add('chat-message', 'bot-message');
                        botMessage.innerText = `HOD of ${selectedBranch}: ${responses[hod of ${selectedBranch.toLowerCase()}]}`;
                        chatBox.appendChild(botMessage);
                        chatBox.scrollTop = chatBox.scrollHeight;

                        currentStep = 0;
                        renderOptions(branches);
                    }
                } else {
                    botMessage.innerText = "Please select an option.";
                    chatBox.appendChild(botMessage);
                }
            } else if (currentStep === 2) {
                const categoryInput = document.querySelector('input[name="category"]:checked');
                if (categoryInput) {
                    selectedCategory = categoryInput.value;

                    // User message
                    const userMessage = document.createElement('div');
                    userMessage.classList.add('chat-message', 'user-message');
                    userMessage.innerText = You selected ${selectedCategory} for ${selectedBranch}.;
                    chatBox.appendChild(userMessage);

                    // Bot message
                    botMessage.classList.add('chat-message', 'bot-message');
                    botMessage.innerText = Please select a gender.;
                    chatBox.appendChild(botMessage);
                    chatBox.scrollTop = chatBox.scrollHeight;

                    currentStep = 3;
                    const genders = Object.keys(cutoffs[selectedBranch][selectedCategory]);
                    renderOptions(genders);
                } else {
                    botMessage.innerText = "Please select a category.";
                    chatBox.appendChild(botMessage);
                }
            } else if (currentStep === 3) {
                const genderInput = document.querySelector('input[name="gender"]:checked');
                if (genderInput) {
                    selectedGender = genderInput.value;

                    // User message
                    const userMessage = document.createElement('div');
                    userMessage.classList.add('chat-message', 'user-message');
                    userMessage.innerText = You selected ${selectedGender} for ${selectedBranch} (${selectedCategory}).;
                    chatBox.appendChild(userMessage);

                    const cutoff = cutoffs[selectedBranch][selectedCategory][selectedGender];
                    // Bot message
                    botMessage.classList.add('chat-message', 'bot-message');
                    botMessage.innerText = The cutoff for ${selectedBranch} (${selectedCategory} - ${selectedGender}) is: ${cutoff};
                    chatBox.appendChild(botMessage);
                    chatBox.scrollTop = chatBox.scrollHeight;

                    currentStep = 0;
                    renderOptions(branches);
                } else {
                    botMessage.innerText = "Please select a gender.";
                    chatBox.appendChild(botMessage);
                }
            } else {
                botMessage.innerText = "Sorry, I didn't understand that. Please ask something else.";
                chatBox.appendChild(botMessage);
            }
        }

        function renderOptions(options) {
            const optionsContainer = document.getElementById('options-container');
            optionsContainer.innerHTML = '';

            if (currentStep === 0) {
                branches.forEach(branch => {
                    const label = document.createElement('label');
                    label.innerHTML = <input type="radio" name="branch" value="${branch}"> <span class="custom-radio"></span>${branch};
                    optionsContainer.appendChild(label);
                });
                const additionalOptions = ["documents required", "fee structure", "important dates"];
                additionalOptions.forEach(option => {
                    const label = document.createElement('label');
                    label.innerHTML = <input type="radio" name="option" value="${option}"> <span class="custom-radio"></span>${option};
                    optionsContainer.appendChild(label);
                });
            } else if (currentStep === 1) {
                options.forEach(option => {
                    const label = document.createElement('label');
                    label.innerHTML = <input type="radio" name="option" value="${option}"> <span class="custom-radio"></span>${option};
                    optionsContainer.appendChild(label);
                });
            } else if (currentStep === 2) {
                options.forEach(option => {
                    const label = document.createElement('label');
                    label.innerHTML = <input type="radio" name="category" value="${option}"> <span class="custom-radio"></span>${option};
                    optionsContainer.appendChild(label);
                });
            } else if (currentStep === 3) {
                options.forEach(option => {
                    const label = document.createElement('label');
                    label.innerHTML = <input type="radio" name="gender" value="${option}"> <span class="custom-radio"></span>${option};
                    optionsContainer.appendChild(label);
                });
            }
        }

        renderOptions(branches);
    </script>
</body>
</html>
