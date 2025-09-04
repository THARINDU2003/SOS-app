<!DOCTYPE html>
<html lang="si">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SOS App</title>
    <!-- Tailwind CSS පුස්තකාලය සම්බන්ධ කිරීම -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+Sinhala:wght@400;700&display=swap');
        body {
            font-family: 'Noto Sans Sinhala', sans-serif;
            background-color: #f3f4f6;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
        }
        .sos-button {
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .sos-button:hover {
            transform: scale(1.05);
            box-shadow: 0 10px 20px rgba(255, 0, 0, 0.4);
        }
        .sos-button:active {
            transform: scale(0.95);
        }
    </style>
</head>
<body>

<div class="bg-white p-8 rounded-3xl shadow-2xl max-w-sm w-full text-center">
    <h1 class="text-3xl font-bold text-gray-800 mb-6">SOS App</h1>
    <p class="text-gray-600 mb-8">අනතුරකදී උදව් ඉල්ලා සිටීමට පහත බොත්තම ඔබන්න.</p>
    
    <!-- SOS බොත්තම -->
    <button id="sosBtn" class="sos-button bg-red-600 text-white font-bold py-6 px-6 rounded-full text-xl shadow-lg hover:bg-red-700 focus:outline-none focus:ring-4 focus:ring-red-300">
        SOS! උදව්
    </button>
    
    <!-- තත්ත්වය පෙන්වන ස්ථානය -->
    <div id="statusMessage" class="mt-8 text-lg font-semibold text-gray-700">
        තත්ත්වය: සූදානම්
    </div>
    <div id="locationInfo" class="mt-4 text-sm text-gray-500 break-words">
        <!-- ස්ථාන තොරතුරු මෙහි පෙන්වනු ඇත -->
    </div>
</div>

<script>
    // අවශ්‍ය HTML මූලද්‍රව්‍ය ලබා ගැනීම
    const sosBtn = document.getElementById('sosBtn');
    const statusMessage = document.getElementById('statusMessage');
    const locationInfo = document.getElementById('locationInfo');

    // SOS බොත්තම එබූ විට සිදුවන දේ
    sosBtn.addEventListener('click', () => {
        // SOS බොත්තම අක්‍රීය කිරීම
        sosBtn.disabled = true;
        sosBtn.classList.add('bg-gray-400');
        sosBtn.classList.remove('bg-red-600', 'hover:bg-red-700', 'focus:ring-red-300');
        
        // තත්ත්ව පණිවිඩය යාවත්කාලීන කිරීම
        statusMessage.textContent = 'තත්ත්වය: ඔබගේ ස්ථානය සෙවීම...';
        locationInfo.textContent = ''; // පැරණි ස්ථාන තොරතුරු ඉවත් කිරීම

        // පරිශීලකයාගේ ස්ථානය ලබා ගැනීම
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(
                (position) => {
                    // ස්ථානය සාර්ථකව ලබා ගත් විට
                    const latitude = position.coords.latitude;
                    const longitude = position.coords.longitude;
                    
                    // තත්ත්ව පණිවිඩය යාවත්කාලීන කිරීම
                    statusMessage.textContent = 'තත්ත්වය: සහය ඉල්ලා සිටින ලදී!';
                    locationInfo.textContent = `ඔබගේ ස්ථානය: අක්ෂාංශ ${latitude}, දේශාංශ ${longitude}`;
                    locationInfo.classList.remove('text-gray-500');
                    locationInfo.classList.add('text-green-600', 'font-bold');

                    // මෙම ස්ථානයේ සිට ඔබට අනතුරු ඇඟවීමේ පණිවිඩය සේවාදායකයකට යැවිය හැකිය.
                    // උදාහරණයක් ලෙස:
                    // sendAlertToServer(latitude, longitude);
                },
                (error) => {
                    // ස්ථානය ලබා ගැනීමේදී දෝෂයක් ඇති වුවහොත්
                    statusMessage.textContent = 'තත්ත්වය: ස්ථානය ලබා ගැනීමට නොහැකි විය.';
                    locationInfo.textContent = 'කරුණාකර යෙදුමට ඔබගේ ස්ථානය භාවිතා කිරීමට අවසර දෙන්න.';
                    locationInfo.classList.remove('text-gray-500');
                    locationInfo.classList.add('text-red-600', 'font-bold');
                    console.error("Geolocation error: ", error.message);
                },
                {
                    enableHighAccuracy: true,
                    timeout: 5000,
                    maximumAge: 0
                }
            );
        } else {
            // Geolocation සහය නොදක්වයි නම්
            statusMessage.textContent = 'තත්ත්වය: Geolocation සහය නොදක්වයි.';
            locationInfo.textContent = 'ඔබගේ උපාංගය ස්ථාන සේවාවන්ට සහය නොදක්වයි.';
            locationInfo.classList.remove('text-gray-500');
            locationInfo.classList.add('text-red-600', 'font-bold');
        }
    });
</script>

</body>
</html>
