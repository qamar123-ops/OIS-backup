<template>
        <nav id="sidebar" class="scrollbar">
            <button type="button" id="sidebarCollapse" class="btn btn-toggle-sidebar">
                <i class="fas fa-align-left"></i>
            </button>
            <ul class="list-unstyled components">
                <li class="active">
                    <a href="#">
                        <span class="lnr lnr-earth"></span>
                        <span class="sb-title">Global Configurations</span>
                    </a>
                </li>
                <li>
                    <a href="#homeSubmenu" data-toggle="collapse" aria-expanded="false" class="dropdown-toggle">
                        <span class="lnr lnr-lock"></span>
                        <span class="sb-title">Authentication Gateways</span>
                    </a>
                    <ul class="collapse list-unstyled" id="homeSubmenu">
                        <li><a href="#">Auth Gateways</a></li>
                        <li><a href="#">Domains</a></li>
                        <li><a href="#">Applications</a></li>
                        <li><a href="#">Attributes</a></li>
                        <li><a href="#">Password Policy</a></li>
                        <li><a href="#">Execution Paths</a></li>
                    </ul>
                </li>
                <li>
                    <a href="#">
                        <span class="lnr lnr-list"></span>
                        <span class="sb-title">Applications Management</span>
                    </a>
                </li>
                <li>
                    <a href="#">
                        <span class="lnr lnr-cog"></span>
                        <span class="sb-title">Application Configurations</span>
                    </a>
                </li>
                <li>
                    <a href="#">
                        <span class="lnr lnr-user"></span>
                        <span class="sb-title">User Management</span>
                    </a>
                </li>
                <li>
                    <a href="#">
                        <span class="lnr lnr-menu"></span>
                        <span class="sb-title">Menu Management</span>
                    </a>
                </li>
                <li>
                    <a href="#">
                        <span class="lnr lnr-link"></span>
                        <span class="sb-title">Navigation Management</span>
                    </a>
                </li>
                <li>
                    <a href="#">
                        <span class="lnr lnr-magnifier"></span>
                        <span class="sb-title">Lookup Management</span>
                    </a>
                </li>
                <li>
                    <a href="#">
                        <span class="lnr lnr-checkmark-circle"></span>
                        <span class="sb-title">Role Management</span>
                    </a>
                </li>
            </ul>
        </nav>
</template>
<script>
</script>