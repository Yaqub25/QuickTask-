# QuickTask-
QuickTask – Get help or earn money by completing tasks. We take 25% commission from each completed job.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QuickTask</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 font-sans">

    <!-- Header -->
    <header class="bg-blue-600 text-white p-6 shadow-lg">
        <div class="max-w-6xl mx-auto flex justify-between items-center">
            <h1 class="text-2xl font-bold">QuickTask</h1>
            <div class="flex items-center gap-4">
                <p id="commissionText" class="text-sm">We take only 25% commission</p>
                <select onchange="changeLanguage(this.value)" class="text-black rounded px-2 py-1">
                    <option value="en">🇬🇧 English</option>
                    <option value="sv">🇸🇪 Svenska</option>
                </select>
            </div>
        </div>
    </header>

    <!-- Hero Section -->
    <section class="text-center py-16 bg-white shadow">
        <h2 id="heroTitle" class="text-4xl font-bold mb-4">Need help? Or want to earn money?</h2>
        <p id="heroText" class="text-lg text-gray-600 mb-6">Post a task or take a job and start immediately.</p>
        <div class="space-x-4">
            <button id="postBtn" onclick="showForm()" class="bg-blue-600 text-white px-6 py-3 rounded-2xl shadow hover:bg-blue-700 transition">Post a Task</button>
            <button id="seeBtn" onclick="scrollToJobs()" class="bg-green-600 text-white px-6 py-3 rounded-2xl shadow hover:bg-green-700 transition">See Jobs</button>
        </div>
    </section>

    <!-- Create Job Form -->
    <section id="jobFormSection" class="max-w-3xl mx-auto mt-10 p-6 bg-white rounded-2xl shadow hidden">
        <h3 id="formTitle" class="text-2xl font-bold mb-4">Create a Task</h3>
        <input id="title" type="text" placeholder="Task Title" class="w-full mb-3 p-3 border rounded-lg">
        <textarea id="description" placeholder="Describe the task" class="w-full mb-3 p-3 border rounded-lg"></textarea>
        <input id="reward" type="number" placeholder="Reward (USD)" class="w-full mb-3 p-3 border rounded-lg">
        <input id="location" type="text" placeholder="Your location (city/postcode)" class="w-full mb-3 p-3 border rounded-lg">
        <button id="publishBtn" onclick="addJob()" class="bg-blue-600 text-white px-6 py-3 rounded-2xl shadow hover:bg-blue-700 transition">Publish</button>
    </section>

    <!-- Job Listings -->
    <section id="jobsSection" class="max-w-6xl mx-auto mt-16 p-6">
        <h3 id="jobsTitle" class="text-3xl font-bold mb-6 text-center">Available Jobs</h3>
        <div id="jobsContainer" class="grid md:grid-cols-2 gap-6"></div>
    </section>

    <!-- Footer -->
    <footer class="bg-gray-800 text-white text-center p-6 mt-16">
        <p id="footerText">© 2026 QuickTask - We take 25% of every completed task.</p>
    </footer>

    <script>
        let currentLang = localStorage.getItem('lang') || 'en';

        function showForm() {
            document.getElementById('jobFormSection').classList.remove('hidden');
            window.scrollTo({ top: document.getElementById('jobFormSection').offsetTop, behavior: 'smooth' });
        }

        function scrollToJobs() {
            window.scrollTo({ top: document.getElementById('jobsSection').offsetTop, behavior: 'smooth' });
        }

        function addJob() {
            const title = document.getElementById('title').value;
            const description = document.getElementById('description').value;
            const reward = parseFloat(document.getElementById('reward').value);
            const loc = document.getElementById('location').value;

            if (!title || !description || !reward || !loc) {
                alert(currentLang === 'en' ? "Please fill in all fields!" : "Fyll i alla fält!");
                return;
            }

            const commission = reward * 0.25;
            const workerGets = reward - commission;

            const jobCard = document.createElement('div');
            jobCard.className = "bg-white p-6 rounded-2xl shadow hover:shadow-lg transition";
            jobCard.dataset.location = loc;
            jobCard.innerHTML = `
                <h4 class="text-xl font-bold mb-2">${title}</h4>
                <p class="text-gray-600 mb-2">${description}</p>
                <p class="font-semibold">${currentLang === 'en' ? 'Reward' : 'Belöning'}: $${reward}</p>
                <p class="text-sm text-gray-500">${currentLang === 'en' ? 'Worker gets' : 'Arbetaren får'}: $${workerGets.toFixed(2)}</p>
                <p class="text-sm text-gray-500" id="distance">Distance: unknown</p>
                <button onclick="this.innerText='${currentLang === 'en' ? 'Taken' : 'Tagen'}'; this.disabled=true" class="mt-3 bg-green-600 text-white px-4 py-2 rounded-xl hover:bg-green-700 transition">${currentLang === 'en' ? 'Take Job' : 'Ta jobbet'}</button>
            `;

            document.getElementById('jobsContainer').appendChild(jobCard);
            updateDistances();

            document.getElementById('title').value = '';
            document.getElementById('description').value = '';
            document.getElementById('reward').value = '';
            document.getElementById('location').value = '';
        }

        function changeLanguage(lang) {
            currentLang = lang;
            localStorage.setItem('lang', lang);
            if (lang === 'sv') {
                document.getElementById('commissionText').innerText = 'Vi tar endast 25% provision';
                document.getElementById('heroTitle').innerText = 'Behöver du hjälp? Eller vill du tjäna pengar?';
                document.getElementById('heroText').innerText = 'Lägg upp ett uppdrag eller ta ett jobb och börja direkt.';
                document.getElementById('postBtn').innerText = 'Lägg upp uppdrag';
                document.getElementById('seeBtn').innerText = 'Se jobb';
                document.getElementById('formTitle').innerText = 'Skapa ett uppdrag';
                document.getElementById('publishBtn').innerText = 'Publicera';
                document.getElementById('jobsTitle').innerText = 'Tillgängliga jobb';
                document.getElementById('footerText').innerText = '© 2026 QuickTask - Vi tar 25% av varje genomfört uppdrag.';
            } else {
                document.getElementById('commissionText').innerText = 'We take only 25% commission';
                document.getElementById('heroTitle').innerText = 'Need help? Or want to earn money?';
                document.getElementById('heroText').innerText = 'Post a task or take a job and start immediately.';
                document.getElementById('postBtn').innerText = 'Post a Task';
                document.getElementById('seeBtn').innerText = 'See Jobs';
                document.getElementById('formTitle').innerText = 'Create a Task';
                document.getElementById('publishBtn').innerText = 'Publish';
                document.getElementById('jobsTitle').innerText = 'Available Jobs';
                document.getElementById('footerText').innerText = '© 2026 QuickTask - We take 25% of every completed task.';
            }
        }

        function updateDistances() {
            if (!navigator.geolocation) return;
            navigator.geolocation.getCurrentPosition(position => {
                const userLat = position.coords.latitude;
                const userLon = position.coords.longitude;
                document.querySelectorAll('#jobsContainer > div').forEach(card => {
                    // Just show as placeholder, in real use we'd geocode the location
                    const loc = card.dataset.location;
                    card.querySelector('#distance').innerText = `${loc} (approx)`;
                });
            });
        }

        // Initialize language
        changeLanguage(currentLang);
    </script>

</body>
</html>
