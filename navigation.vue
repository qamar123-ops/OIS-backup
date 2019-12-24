<template>
	<div class="wrapper">
		<nav class="navbar navbar-expand-lg navbar-light">
			<div class="container-fluid">
				<div class="brand-header">
					<h3>OIS Framework</h3>
					<strong>OIS</strong>
				</div>
				<button class="navbar-toggler" type="button" data-toggle="collapse"
					data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false"
					aria-label="Toggle navigation">
					<span class="navbar-toggler-icon"></span>
				</button>
				<div class="collapse navbar-collapse" id="navbarSupportedContent">
					<ul class="navbar-nav ml-auto">
						<li class="nav-item active">
							<a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
						</li>
						<li class="nav-item">
							<a class="nav-link" href="#">Link</a>
						</li>
						<li class="nav-item dropdown">
							<a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button"
								data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
								Dropdown
							</a>
							<div class="dropdown-menu" aria-labelledby="navbarDropdown">
								<a class="dropdown-item" href="#">Action</a>
								<a class="dropdown-item" href="#">Another action</a>
								<div class="dropdown-divider"></div>
								<a class="dropdown-item" href="#">Something else here</a>
							</div>
						</li>
						<li class="nav-item">
							<a class="nav-link disabled" href="#">Disabled</a>
						</li>
					</ul>

				</div>
				<div class="user-details-section float-right">
					<div class="dropdown user-d-dropdown">
						<div class="dropdown-toggle d-flex align-items-center" data-toggle="dropdown">
							<div class="avatar float-left">
								<span class="lnr lnr-user"></span>
							</div>
							<div class="username"><span class="highlight">Hello!</span> User Name,
								<span class="userrole">Admin User,</span>
							</div>
						</div>
						<div class="dropdown-menu">
							<a class="dropdown-item" href="#">Link 1</a>
							<a class="dropdown-item" href="#">Link 2</a>
							<a class="dropdown-item" href="#">Link 3</a>
						</div>
					</div>
				</div>
			</div>
		</nav>
	</div>
</template>

<script>
	module.exports = {};
</script>

<style scoped>

</style>