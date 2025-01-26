

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8" />
	<title>eLabRate</title>
	<!-- Bootstrap CSS -->
	<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet" />
	<!-- Font Awesome for icons -->
	<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
	<style>
		body {
			background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
			font-family: 'Arial', sans-serif;
			margin: 0;
			padding: 0;
			color: #333;
		}

		header {
			background: linear-gradient(to right, #007bff, #0056b3);
			color: #fff;
			padding: 2rem 1rem;
			text-align: center;
			position: relative;
			box-shadow: 0 4px 6px rgba(0,0,0,0.1);
			transition: all 0.3s ease;
		}

			header:hover {
				transform: translateY(-5px);
				box-shadow: 0 6px 12px rgba(0,0,0,0.15);
			}

			header h1 {
				margin: 0;
				font-size: 6rem;
				font-weight: 700;
				text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
			}

		.logo-img {
			position: absolute;
			top: 50%;
			left: 5rem;
			transform: translateY(-50%) rotate(0deg);
			width: 100px;
			height: 80px;
			border-radius: 50%;
			border: 3px solid #fff;
			box-shadow: 0 4px 6px rgba(0,0,0,0.2);
			transition: transform 0.5s ease;
		}

			.logo-img:hover {
				transform: translateY(-50%) rotate(360deg);
			}

        .search-bar {
            margin-top: 2rem;
            margin-bottom: 2rem;
            perspective: 1000px;
        }

		#searchInput {
			border-radius: 20px;
			padding: 10px 20px;
			transition: all 0.3s ease;
			box-shadow: 0 2px 4px rgba(0,0,0,0.1);
		}

			#searchInput:focus {
				transform: scale(1.02);
				box-shadow: 0 4px 8px rgba(0,0,0,0.2);
				border-color: #007bff;
			}

		.lab-card {
			background-color: #fff;
			border: none;
			border-radius: 15px;
			padding: 1.5rem;
			margin-bottom: 1.5rem;
			box-shadow: 0 4px 6px rgba(0,0,0,0.1);
			transition: all 0.3s ease;
			position: relative;
			overflow: hidden;
		}

			.lab-card:hover {
				transform: translateY(-10px);
				box-shadow: 0 8px 15px rgba(0,0,0,0.2);
			}

			.lab-card::before {
				content: '';
				position: absolute;
				top: 0;
				left: 0;
				width: 100%;
				height: 5px;
				background: linear-gradient(to right, #007bff, #0056b3);
				transform: scaleX(0);
				transform-origin: left;
				transition: transform 0.3s ease;
			}

			.lab-card:hover::before {
				transform: scaleX(1);
			}

			.lab-card h3 {
				color: #007bff;
				margin-bottom: 0.75rem;
				font-weight: 600;
			}

			.lab-card .badge {
				position: absolute;
				top: 10px;
				right: 10px;
				background-color: #28a745;
				color: white;
			}

		footer {
			background: linear-gradient(to right, #f8f9fa, #e9ecef);
			padding: 1.5rem;
			text-align: center;
			box-shadow: 0 -4px 6px rgba(0,0,0,0.05);
		}

		@keyframes fadeIn {
			from {
				opacity: 0;
				transform: translateY(20px);
			}

			to {
				opacity: 1;
				transform: translateY(0);
			}
		}

		.lab-card {
			animation: fadeIn 0.5s ease forwards;
			opacity: 0;
		}
	</style>
</head>
<body>
	<header>
		<img src="logo/logo_v0.png" alt="eLabRate Logo" class="logo-img" />
		<h1>eLabRate</h1>
		<p>Your gateway to university lab information</p>
	</header>

	<main class="container">
		<div class="row search-bar">
			<div class="col-md-6 offset-md-3">
				<input type="text"
					   id="searchInput"
					   class="form-control"
					   placeholder="🔍 Search labs by name or university..."
					   oninput="filterLabs()" />
			</div>
		</div>

		<div class="row" id="labContainer">
			<!-- Labs will be dynamically inserted here -->
		</div>
	</main>

	<footer>
		<p>
			<i class="fas fa-university"></i> © 2025 eLabRate. All rights reserved.
			<span id="labCount" class="badge bg-info ms-2"></span>
		</p>
	</footer>

	<script>
		const labsData = [
			{
				university: "JHU",
				labName: "Adam D. Sylvester Lab",
				description: "anatomy, biomechanics, evolution, locomotion, skeletal morphology",
				URL: "https://www.hopkinsmedicine.org/research/labs/a/adam-d-sylvester-lab",
				tags: ["Anatomy", "Biomechanics"]
			},
			{
				university: "JHU",
				labName: "Adam Sapirstein Lab",
				description: "brain injury, brain, excitotoxicity, lipid metabolites, neurological disorders, phospholipases A2, stroke",
				URL: "https://www.hopkinsmedicine.org/research/labs/a/adam-sapirstein-lab",
				tags: ["Neurology", "Brain Research"]
			},
			{
				university: "JHU",
				labName: "Adamo Cardiac Immunology Lab",
				description: "cardiac function and dysfunction, heart disease, immunology",
				URL: "https://www.hopkinsmedicine.org/research/labs/a/adamo-cardiac-immunology-lab",
				tags: ["Cardiology", "Immunology"]
			},
			{
				university: "JHU",
				labName: "Adrian Dobs Lab",
				description: "cardiovascular diseases, diabetes mellitus, endocrinology, hormones, hyperlipidemia, male gonadal function",
				URL: "https://www.hopkinsmedicine.org/research/labs/a/adrian-dobs-lab",
				tags: ["Endocrinology", "Cardiovascular"]
			}
		];

		function renderLabs(labs) {
			const container = document.getElementById('labContainer');
			container.innerHTML = "";

			labs.forEach((lab, index) => {
				const card = document.createElement('div');
				card.classList.add('col-md-6');
				card.style.animationDelay = `${index * 0.1}s`;

				const tagBadges = lab.tags.map(tag =>
					`<span class="badge bg-secondary me-1">${tag}</span>`
				).join('');

				card.innerHTML = `
					<div class="lab-card">
						<h3>${lab.labName}</h3>
						<p><strong><i class="fas fa-university"></i> University:</strong> ${lab.university}</p>
						<p><strong><i class="fas fa-info-circle"></i> Description:</strong> ${lab.description}</p>
						<p><strong><i class="fas fa-link"></i> Contact:</strong>
							<a href="${lab.URL}" target="_blank">Lab Website</a>
						</p>
						<div class="tags mt-2">${tagBadges}</div>
					</div>
				`;
				container.appendChild(card);
			});

			document.getElementById('labCount').textContent =
				`${labs.length} Labs Found`;
		}

		function filterLabs() {
			const searchValue = document
				.getElementById('searchInput')
				.value
				.toLowerCase();

			const filtered = labsData.filter((lab) => {
				return (
					lab.labName.toLowerCase().includes(searchValue) ||
					lab.university.toLowerCase().includes(searchValue) ||
					lab.tags.some(tag => tag.toLowerCase().includes(searchValue))
				);
			});

			renderLabs(filtered);
		}

		document.addEventListener('DOMContentLoaded', function () {
			renderLabs(labsData);
		});
	</script>
</body>
</html>
