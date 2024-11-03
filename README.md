<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NID Information Fetcher</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 500px;
            margin: auto;
            padding: 20px;
            background-color: #f3f3f3;
        }
        .container {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        label {
            display: block;
            margin-top: 10px;
            font-weight: bold;
        }
        input[type="text"] {
            width: 100%;
            padding: 8px;
            margin-top: 5px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        button {
            margin-top: 15px;
            padding: 10px 20px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        button:disabled {
            background-color: #ccc;
        }
        .response {
            margin-top: 20px;
            padding: 10px;
            background-color: #e9f7ef;
            border: 1px solid #d4edda;
            border-radius: 4px;
            color: #155724;
        }
        .error {
            color: #721c24;
            background-color: #f8d7da;
            border: 1px solid #f5c6cb;
            padding: 10px;
            margin-top: 10px;
            border-radius: 4px;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>NID Information</h2>
    <label for="nid">NID No.</label>
    <input type="text" id="nid" placeholder="Enter your NID number">

    <label for="dob">DOB</label>
    <input type="text" id="dob" placeholder="Enter your Date of Birth (YYYY-MM-DD)">

    <button onclick="fetchNIDInfo()">ডাউনলোড করুন</button>

    <div id="response" class="response" style="display: none;"></div>
    <div id="error" class="error" style="display: none;"></div>
</div>

<script>
    async function fetchNIDInfo() {
        const nid = document.getElementById('nid').value.trim();
        const dob = document.getElementById('dob').value.trim();
        const responseDiv = document.getElementById('response');
        const errorDiv = document.getElementById('error');
        
        // Clear previous responses
        responseDiv.style.display = 'none';
        errorDiv.style.display = 'none';

        // Validate inputs
        if (!nid || !dob) {
            errorDiv.innerHTML = "অনুগ্রহ করে NID নম্বর এবং জন্ম তারিখ প্রদান করুন।";
            errorDiv.style.display = 'block';
            return;
        }

        try {
            const apiURL = `http://api.blackfiretools.my.id/server/pdf/GR-Mane.php?nid=${nid}&dob=${dob}`;
            const response = await fetch(apiURL);

            if (response.ok && response.headers.get('Content-Type') === 'application/pdf') {
                const blob = await response.blob();
                const url = window.URL.createObjectURL(blob);

                responseDiv.innerHTML = `<a href="${url}" download="${nid}_info.pdf">পিডিএফ ডাউনলোড করুন</a>`;
                responseDiv.style.display = 'block';
            } else {
                throw new Error("তথ্য পাওয়া যায়নি বা পিডিএফ তৈরিতে ত্রুটি ঘটেছে। দয়া করে সঠিক তথ্য প্রদান করুন।");
            }
        } catch (error) {
            errorDiv.innerHTML = error.message;
            errorDiv.style.display = 'block';
        }
    }
</script>

</body>
</html>
